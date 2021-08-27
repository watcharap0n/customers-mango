# CHATBOT Multiple Channel Flask

`What Flask?`

    Flask is a lightweight WSGI web application framework. 
    It is designed to make getting started quick and easy, 
    with the ability to scale up to complex applications. 
    It began as a simple wrapper around Werkzeug and Jinja
    and has become one of the most popular Python web application frameworks.
    Flask offers suggestions, but doesn't enforce any dependencies or project layout. 
    It is up to the developer to choose the tools and libraries they want to use.
    There are many extensions provided by the community that make adding new functionality easy.

`Features`

    - Flask-RESTPlus and Swagger UI
    - LINE Official
    - LINE Developer
    - Facebook Developer
    - API From Public WEB Mango Consultant 
    - Concept Design Patterns (MVC)

****

- Main Library
    - flask
    - flask-swagger
    - scikit-learn
    - pytorch
    - attacut
    - line-bot-sdk
    - pymessager
    - pymongo
    - firebase_admin
    - pyrebase4

**Webhook Callback can create Channel LINE your self**

```python
    @route_callback.route('/callback/<string:token>', methods=['POST'])
@api.validate(tags=['Callback'])
def webhook(token):
    raw_json = request.get_json()
    q = db.find_one(collection=collection, query={'token': token})
    q = dict(q)
    handler = q['SECRET_LINE']
    handler = WebhookHandler(handler)
    with open('static/line_log.json', 'w') as log_line:
        json.dump(raw_json, log_line)
    try:
        signature = request.headers['X-Line-Signature']
        body = request.get_data(as_text=True)
        events = raw_json['events'][0]
        _type = events['type']
        if _type == 'follow':
            userId = events['source']['userId']
            profile = get_profile(userId, q)
            inserted = {'displayName': profile['displayName'], 'userId': userId, 'img': profile['img'],
                        'status': profile['status']}
            db.insert_one(collection='line_follower', data=inserted)
        elif _type == 'unfollow':
            userId = events['source']['userId']
            db.delete_one('line_follower', query={'userId': userId})
        elif _type == 'postback':
            event_postback(events, q)
        elif _type == 'message':
            message_type = events['message']['type']
            if message_type == 'text':
                try:
                    userId = events['source']['userId']
                    message = events['message']['text']
                    profile = get_profile(userId, q)
                    push_message = {'user_id': userId, 'message': message, 'display_name': profile['displayName'],
                                    'img': profile['img'],
                                    'status': profile['status'], 'access_token': q['ACCESS_TOKEN']}
                    db.insert_one(collection='message_user', data=push_message)
                    handler.handle(body, signature)
                    handler_message(events, q)
                except InvalidSignatureError as v:
                    api_error = {'status_code': v.status_code, 'message': v.message}
                    return jsonify(api_error), 400
            else:
                no_event = len(raw_json['events'])
                for i in range(no_event):
                    events = raw_json['events'][i]
                    event_handler(events, q)
    except IndexError:
        return jsonify({'index': None}), 200
    return jsonify(raw_json)

```

**Build && Setup Python**
**Build && Setup Python**

~~~~
$ pip install virtualenv
~~~~

~~~~
$ virtualenv venv
~~~~

~~~~
$ source venv/bin/activate
~~~~

~~~~
$ (venv) pip install -r requirements.txt
~~~~

MAC requirement
~~~~
$ brew tap mongodb/brew
$ brew install mongodb-community@4.4
$ brew install --cask robo-3t
 ~~~~

~~~~
$ sudo mongod --dbpath /usr/local/var/mongodb
~~~~

**Deploy On Heroku**

~~~~
$ heroku login
$ heroku git:clone -a (repo-name)
$ cd game-card-watcharapono
$ git add .
$ git commit -am "make it better"
$ git push heroku master
 ~~~~


