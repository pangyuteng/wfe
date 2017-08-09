# wfe - work flow engine

# integrate the below

# # deployment of a work flow engine
Producer
	- listens to actions
		- postgres: notify and listen
		- anything else, other db, file io, rest api?
	- produces tasks depending on a trigger logic using the info from the actions.
		- must be configurable
		- tasks is pushed to prespecified queue.
Queues:
	- queues, likely implmented with Celery and RabbitMq as backend messaging queue.
Tasks:
	- Tasks are distributed to queues based on average run time of tasks.
	- Emulate high latency tasks to be sent to HTC Condor for processing.
	- Emulate low latency tasks with random time.sleep.
	- Tasks shall impl Luigi, with varying output, MongoDBCellTarget and FileTarget.
Consumer:
	- one worker consuming long running tasks, emulates sending to cluster with different OS if necessary.
		- uses Luigi to break up the tasks in to smaller tasks.
		- thus between smaller tasks, there  could be pauses/waits, even for human interaction.
	- other worker (s) can consume low latency tasks from
Web:
	- persist state to mongodb for each task
	- displayed the workflow in web page.
Testing:
	- write test cases
Depolyment
	- use supervisord and crontab to restart workers?
	- use something like PM2 to install and start all services including admin gui servers
		- rabbitmq
		- flower
		- luigi
		- condor?
	- create script
		- docker or vagrant or amazon