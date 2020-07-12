---
layout: post
title:  "Processing Large Tasks with Laravel Queues"
date:   2020-07-12  11:37:17 -0700
categories: software engineering
author: Tyler Souza
share: true
---

Recently, I tackled an interesting problem with an app I’m building.  Just for context and without going into too much detail, this system is a management admin for guidance counselors to help manage their patients who are students in High School. I had to build a feature that generated five surveys with incrementing deadlines every time a new client was entered into the system. Easy enough right? Well, every survey generated a number of questions and categories per survey. This could mean hundreds of questions could be created at one time, thus slowing down the response time since everything would be running synchronously and waiting for all of the insert queries to complete. It was obvious that I couldn’t just create those records right in the controller, I had to process those tasks in the background. 

## Laravel Queues to the Rescue!
I’ve been building this app using [Laravel](https://laravel.com/) for a couple of years and it’s been pretty stable in production. I’ve gotta say, I’m pretty happy with it. the past, I’ve used [Celery](https://github.com/celery/celery), which is a a job queue in Python or something like [Sidekiq](https://moove-it.github.io/sidekiq-scheduler/) to process tasks in the background. I’ve even setup a simple job queue in Go. However, I’ve never had the opportunity to use Laravel’s queue system. 

It was actually the easiest tool I’ve ever has to setup! One of the many reasons why I love Laravel as a framework. Let’s go over my solution to the survey problem and then in end go over setup. 

First I created a queue called `ProcessDeadline`.  I passed injected the `Client` model and passed in an array (which will hold data for our deadline) in the constructor. Then in the `handle()` method, I created a new instance to my `SurveyFactory`  classand called `generateSurveyDeadline` that will be called everything the `dispatch()` method is fired somewhere in the code base; in our case it will be in the `ClientController`. SurveyFactory handles the creation of a survey. 

```
<?php

namespace App\Jobs;

use App\Client;
use App\Components\Survey\SurveyFactory;
use Illuminate\Bus\Queueable;
use Illuminate\Queue\SerializesModels;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;

class ProcessDeadline implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    protected $preparedDeadline;

    /**
     * Create a new job instance.
     *
     * @return void
     */
    public function __construct(Client $client, array $preparedDeadline)
    {
        $this->client = $client;
        $this->preparedDeadline = $preparedDeadline;
    }

    /**
     * Execute the job.
     *
     * @return void
     */
    public function handle()
    {

        $surveyFactory = new SurveyFactory();
        $surveyFactory->generateSurveyDeadline($this->preparedDeadline);
    }
}

```

I have a `DeadlineService` that is purposely coupled to a `Client` model. I created a `preparedDeadlines` array in the constructor.

```
<?php

// ....

class DeadlineService
{
    public function __construct(Client $client)
    {   
        $this->client = $client;
        $this->preparedDeadlines = [
            [
                'section'       => 'Intake', 
                'counselor_id'  => $this->client->counselor_id,
                'survey_type'   => 'caloms-juvenile-intake',
                'deadline'      => self::INIT_INTAKE_DEADLINE,
                'intake_date'   => $this->client->intake_date,
                'client_id'     => $this->client->id     
            ],
			// ... shortened for brevity...
    }
}
```

Within that service class, I have a `activateClientDeadline` method that actually dispatches as `ProcessDeadline` within an foreach loop. Which has a delay of 10 seconds for each deadline.

```
    <?php>

    // ...

    /**
     * Activates deadlines on client creation.
     * or activates when client is re-activated.
     *
     * @return void
     */
    public function activateClientDeadlines()
    {
        // loop through pendingDeadlines array
        foreach($this->preparedDeadlines as $pd) {
            // dispatch jobs instead
            ProcessDeadline::dispatch($this->client, $pd)
                ->delay(now()->addSeconds());
        }
    }
``` 

Note, I could’ve had this in the controller, but it made since to have this in a service class, since I’ve already been using it to handle deadlines and deadline data in the system. 

Next, we can call are activate method in the deadline service in our store method in the `ClientController`.

```
    <?php

    // ... 

    public function store(Request $request)
    {
        
        $validatedData = $request->validate([
            'first_name'    => 'required',
            'last_name'     => 'required',
            'email'         => 'required|email',
            'phone'         => 'required',
            'age'           => 'required',
            'birth_date'    => 'required',
            'client_id'     => 'required',
            'counselor_id'  => 'required',
            'intake_date'   => 'required|date'
        ]);

        $client = Client::create($request->all());

        // create deadlines for new client
        $deadlineService = new DeadlineService($client);
        $deadlineService->activateClientDeadlines();


        return redirect()->route('clients')->withInput()->with('New client saved');
    }
```

And boom!  Simple enough! We are now processing jobs in the background. 

## Server Setup 
Server setup was also extremely easy! Laravel docs really are great: [Queues - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/7.x/queues). First Laravel requires to setup migrations that will create jobs table in the database what will hold jobs (also failed jobs). So I had to set that up.

```
php artisan queue:table

php artisan migrate

```

I used Redis as my queue driver. Setup as a running process through systemd on my Ubuntu VPS instance. 

Next, I configured supervisor to keep `queue:work` running on my server and to restart the process if it fails. Installed via apt-get `sudo apt-get install supervisor`.

Then setup my `app-worker.conf` config file in `/etc/supervisor/conf.d`.

```
[program:app-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/apps/app.com/artisan queue:work redis --sleep=3 --tries=3
autostart=true
autorestart=true
user=tyler
numprocs=8
redirect_stderr=true
stdout_logfile=/home/apps/app.com/worker.log
stopwaitsecs=3600
```

Then update the supervisor config and start the process.

```
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl start app-worker:*
```

We all set now! When creating a new Client, the response time is instant, while those surveys get processed in the background. Super easy!

Also a thing to note, I running this app in a cheap DigitalOcean VM server with 1 CPU core and 1GB of RAM. This queue setup barely takes up any resources when processing, which is nice. I should setup some benchmarks on this! 
