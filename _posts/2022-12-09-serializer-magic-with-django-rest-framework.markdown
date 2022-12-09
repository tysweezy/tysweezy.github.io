---
layout: post
title:  "Serializer Magic with Django REST Framework"
date:   2022-12-09  09:21:17 -0700
categories: software engineering
author: Tyler Souza
share: true
---


Currently, I am on a quest to build a social network for developers and using the Django REST Framework to build out the API backend. While working on the anonymous feed endpoint I came accross this in the ListView

```json

// under a results object... 


        {
            "content": "I am a bacon person",
            "content_type": "text",
            "user": 1,
            "post_type": "original",
            "created": "2022-11-17T00:42:59.237315Z"
        },

```



This is great, but this is not really what I want. I want to also list the user/poster details instead of just the user id. Lets get into our serializers to make this happen! You can add in another serializer as a field in the serializer that you're using. In my case, I created a model serializer called `UserInfoSerializer` for listing out basic user information. I then used that serializer as a field in the `StandardPostSerilizer` which is a `read_only` field.  Here is an example: 

```python

# from accounts/serializer.py
class UserInfoSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ('id', 'username', 'email')


# the serializer class I am using
# posts/serializer.py
class StandardPostSerializer(serializers.ModelSerializer):
    user = UserInfoSerializer(read_only=True)

    class Meta:
        model = Post
        fields = ['content', 'content_type', 'user', 'post_type', 'created']


# my anon post feed view
# posts/views.py
class AnonFeedView(generics.ListAPIView):
    """
    public newsfeed endpoint. Any user can access this feed 
    without a registered account.
    """
    serializer_class = StandardPostSerializer
    permission_classes = [permissions.AllowAny]
    queryset = Post.objects.all()

```


Boom! I now have post results with poster (user) details attached. 

```json

**HTTP 200 OK**
**Allow:** GET, HEAD, OPTIONS
**Content-Type:** application/json
**Vary:** Accept

{
    "count": 20,
    "next": null,
    "previous": null,
    "results": [
        {
            "content": "I am a bacon person",
            "content_type": "text",
            "user": {
                "id": 1,
                "username": "tyler",
                "email": "example@email.com"
            },
            "post_type": "original",
            "created": "2022-11-17T00:42:59.237315Z"
        },
        {
            "content": "What is life my dude",
            "content_type": "text",
            "user": {
                "id": 1,
                "username": "tyler",
                "email": "example@email.com"
            },
            "post_type": "reply",
            "created": "2022-11-17T06:22:21.864202Z"
        },
	    // .....
    ]
}
```


Here are some guides --

* [Custom Serializer Fields](https://www.django-rest-framework.org/api-guide/fields/#custom-fields)
* [Model Relationships Django REST Framework](https://corgibytes.com/blog/2022/06/14/model-relationships-django-rest-framework/)
* [AllowAny Permission Class and DRF Permission Guides](https://www.django-rest-framework.org/api-guide/permissions/#allowany)
* [ListAPIView and Other Generic Views](https://www.django-rest-framework.org/api-guide/generic-views/#listapiview)
