How to Deploy a Dockerized Laravel App to AWS
=============================================

* ***Actions on the deployment of the project:***

- Making a new project aws-laravel_sscripts.loc:

	sudo chmod -R 777 /var/www/LARAVEL/AWS/aws-laravel_sscripts.loc

	//!!!! .conf
	sudo cp /etc/apache2/sites-available/test.loc.conf /etc/apache2/sites-available/aws-laravel_sscripts.loc.conf
		
	sudo nano /etc/apache2/sites-available/aws-laravel_sscripts.loc.conf

	sudo a2ensite aws-laravel_sscripts.loc.conf

	sudo systemctl restart apache2

	sudo nano /etc/hosts

	cd /var/www/LARAVEL/AWS/aws-laravel_sscripts.loc

- Deploy project:

	`git clone << >>`
	
	`or Download`
	
	_+ Сut the contents of the folder up one level and delete the empty one._

	`composer install`	

---

Scalable Scripts																				

[How to Deploy a Dockerized Laravel App to AWS (13:09)]( https://www.youtube.com/watch?v=_EG6ytpIbAI&ab_channel=ScalableScripts )

Learn how to create a production-ready Docker image with Laravel. Then we will use that Docker Image to push it to AWS Container Registry and we will Host it to AWS Fargate.

Check more tutorials: 

React and Laravel: Breaking a Monolith to Microservices: 
<https://scalablescripts.com/p/react-laravel-microservices>

Vue 3 and Laravel: Breaking a Monolith to Microservices: 
<https://scalablescripts.com/p/react-laravel-microservices>

Angular and Laravel: Breaking a Monolith to Microservices: 
<https://scalablescripts.com/p/angular-laravel-microservices>

Laravel RESTful APIs Advanced: Docker, Redis, Stripe: 
<https://scalablescripts.com/>

Get access to all my courses for 15$/month: 
<https://scalablescripts.com/p/membership>

[(0:00)]( https://youtu.be/_EG6ytpIbAI?t=0 )

	composer create-project --prefer-dist laravel/laravel

[(3:15)]( https://youtu.be/_EG6ytpIbAI?t=195 ) `Create a Dockerfile`.

```
FROM php:7.4-fpm

RUN apt-get update && apt-get install -y \
	git \
	curl \
	libpng-dev \
	libonig-dev \
	libxml2-dev \
	zip \
	unzip
	
RUN curl -sS https://getcomposer.org//installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN docker-php-ext-install pdo_mysql mbstring

WORKDIR /app
COPY composer.json .
RUN composer install --no-scripts
COPY . .

CMD php artisan serve --host=0.0.0.0 --port 80
```

	docker build -t app .

[(3:53)]( https://youtu.be/_EG6ytpIbAI?t=233 ) `Run Docker`.

	docker run -p 8888:80 app
	
![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/terminal/1.png )	

[(4:05)]( https://youtu.be/_EG6ytpIbAI?t=245 ) `In Browser`:

	https://localhost:8888
	
![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/firefox/1.png )

`Let's push this container to the cloud.`

[(4:20)]( https://youtu.be/_EG6ytpIbAI?t=260 ) `AWS Management Console`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/1.png )

[(4:30)]( https://youtu.be/_EG6ytpIbAI?t=270 ) `Elastic Container Registry`.

[(5:10)]( https://youtu.be/_EG6ytpIbAI?t=310 ) `AWS CLI`.

	https://aws.amazon.com/cli/

	sudo apt  install awscli   # version 1.18.69-1ubuntu0.20.04.1

[(5:25)]( https://youtu.be/_EG6ytpIbAI?t=310 ) `In Terminal`:

	aws 
	
![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/terminal/2.png )

`Login to Elastic Container Registry by using this command.`

	aws ecr get-login-password --region eu-north-1 | docker login --username AWS --password-stdin 522751208087.dkr.ecr.eu-north-1.amazonaws.com
	
Error:	
<https://www.changwoo.org/x1wins@changwoo.net/2021-02-12/An-error-occurred-when-calling-the-GetAuthorizationToken-342d3fb68c>
		
[(6:40)]( https://youtu.be/_EG6ytpIbAI?t=400 ) `What is left is to Push our Container to the Elastic Container Registry`.

[(7:25)]( https://youtu.be/_EG6ytpIbAI?t=445 ) 

	docker tag app 522751208087.dkr.ecr.eu-north-1.amazonaws.com/app
	
	docker push 522751208087.dkr.ecr.eu-north-1.amazonaws.com/app

_"Got some error:"_

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/terminal/3.png )

[(7:45)]( https://youtu.be/_EG6ytpIbAI?t=465 ) `Let's Create Repository`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/2.png )

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/3.png )

[(8:15)]( https://youtu.be/_EG6ytpIbAI?t=495 ) `Pushing`.

	docker push 522751208087.dkr.ecr.eu-north-1.amazonaws.com/app

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/terminal/4.png )

[(8:30)]( https://youtu.be/_EG6ytpIbAI?t=510 ) `Images`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/4.png )

[(8:45)]( https://youtu.be/_EG6ytpIbAI?t=525 ) `Private Repository. Copy URI`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/3.png )

[(8:55)]( https://youtu.be/_EG6ytpIbAI?t=535 ) `Amazon ECS( Elastic Container Service ). Create Cluster`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/5.png )

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/6.png )

`Next step`.

[(9:30)]( https://youtu.be/_EG6ytpIbAI?t=570 ) `Configure cluster`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/7.png )

`Create`.

[(9:48)]( https://youtu.be/_EG6ytpIbAI?t=588 ) `Launch status`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/8.png )

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/9.png )

[(9:55)]( https://youtu.be/_EG6ytpIbAI?t=595 ) `Task Definitions-> Create new Task Definition. Select FARGATE`. 

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/10.png )

`Next step`.

[(10:10)]( https://youtu.be/_EG6ytpIbAI?t=610 ) `Configure task and container definitions`. 

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/11.png )

[(10:22)]( https://youtu.be/_EG6ytpIbAI?t=622 ) `Task size`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/12.png )

[(10:40)]( https://youtu.be/_EG6ytpIbAI?t=640 ) `Add Container`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/13.png )

`Add`.

`Create Task Definition`.

[(11:00)]( https://youtu.be/_EG6ytpIbAI?t=660 ) `View Task Definition`.

[(11:05)]( https://youtu.be/_EG6ytpIbAI?t=665 ) `Clusters-> MyAPP. Create( Service )`. 

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/14.png )

[(11:15)]( https://youtu.be/_EG6ytpIbAI?t=675 ) `Configure service`. 

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/15.png )

`Next step`.

[(12:00)]( https://youtu.be/_EG6ytpIbAI?t=720 ) `Configure Network`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/16.png )

`Next step`.

[(12:10)]( https://youtu.be/_EG6ytpIbAI?t=730 ) `Set Auto Scaling(optional) `.

`Next step. Review`.

`Create Service. View Service`.

[(12:30)]( https://youtu.be/_EG6ytpIbAI?t=750 ) `Set Auto Scaling(optional) `.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/17.png )

`Status PENDING`.

[(12:50)]( https://youtu.be/_EG6ytpIbAI?t=770 ) `Task is RUNINNG now. And we have a Public IP for it`.

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/aws/18.png )

[(12:55)]( https://youtu.be/_EG6ytpIbAI?t=775 ) `Copy Public IP and Paste in Browser`.

_"So this is our App."_

![screenshot of sample]( https://github.com/mslobodyanyuk/aws-laravel_sscripts/blob/main/public/images/firefox/2.png )

#### Useful links:

Scalable Scripts																				

[How to Deploy a Dockerized Laravel App to AWS]( https://www.youtube.com/watch?v=_EG6ytpIbAI&ab_channel=ScalableScripts )

Possible Errors

<https://www.changwoo.org/x1wins@changwoo.net/2021-02-12/An-error-occurred-when-calling-the-GetAuthorizationToken-342d3fb68c>