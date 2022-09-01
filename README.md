# Asynchronous Tasks with FastAPI and Celery

Example of how to handle background processes with FastAPI, Celery, and Docker.

## Want to learn how to build this?

Check out the [post](https://testdriven.io/blog/fastapi-and-celery/).

## Want to use this project?

Spin up the containers:

```sh
$ docker-compose up -d --build
```

Open your browser to [http://localhost:8004](http://localhost:8004) to view the app or to [http://localhost:5556](http://localhost:5556) to view the Flower dashboard.

Trigger a new task:

```sh
$ curl http://localhost:8004/tasks -H "Content-Type: application/json" --data '{"type": 0}'
```

Check the status:

```sh
$ curl http://localhost:8004/tasks/<TASK_ID>
```

To Check the fastAPI endpoint docs, open your browser to [http://localhost:8004/docs](http://localhost:8004/docs). 


Now try Posting to the API. In the `/tasks` endpoint Request body you can paste `{"type": 1}` and click Execute. Look in project/logs/celery.log and you will see

```
[2022-09-01 19:20:02,890: INFO/MainProcess] Received task: create_task[576dd7c0-3ad4-4bd0-852a-8b39e57424a1]  
[2022-09-01 19:20:04,371: INFO/ForkPoolWorker-1] Task create_task[5d659c4a-f23e-49e8-9524-5b1a5b99ca93] succeeded in 10.013130604999787s: True
```

thats becasue by sending `{"type": 1}` you caused there to be a 1*10 second task, as you can see from looking in project/worker.py

```
@celery.task(name="create_task")
def create_task(task_type):
    time.sleep(int(task_type) * 10)
    return True
```