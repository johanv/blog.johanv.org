<!--
.. title: gitlab-ci, codeception and selenium web tests
.. slug: gitlab-ci-codeception-and-selenium-web-tests
.. date: 2019-05-16 07:27:33 UTC+02:00
.. tags: tests,ci
.. category: 
.. link: 
.. description: 
.. type: text
-->

All the php projects I work on, have tests. For some projects, we use [PHPUnit](https://phpunit.de) as testing framework, but for more recent projects, I prefer to use [Codeception](https://codeception.com), because it has an elegant way to write acceptance tests. We also use gitlab for our projects, and by adding a nifty `.gitlab-ci.yml` file to our source code, we made [gitlab run our test suite](https://about.gitlab.com/product/continuous-integration/) every time new commits are pushed to the repository.

Recently, we started using [Selenium WebDriver](https://codeception.com/docs/modules/WebDriver) with codeception, so that we could run browser tests, to make sure that the javascripts we use keep working. Of course we wanted to run these browser tests with gitlab-ci as well, but that was not as easy as we thought it would be. The idea was to run selenium with a browser [via a docker container](https://codeception.com/docs/modules/WebDriver#Headless-Selenium-in-Docker), but the challenging part was setting up a webserver the browser could connect to.

In a first attempt, I tried to start all the containers needed for our application, running `docker-compose up` from the ci like we do it for local development. This worked sometimes, but after a couple of builds the docker networks used by our gitlab runners got completely messed up, which was hard to fix.

In a second attempt, I tried to use [Gitlab CI Services](https://docs.gitlab.com/ee/ci/services/) for all the containers in our `docker-compose.yml` file. I gave up before I completely configured that, because I noticed the networking problems wouldn't go away.

After days of swearing, I finally got it running. I think the networking problems were caused by the nginx-container I tried to use for the web tests. But actually I didn't need a container for nginx. It turned out I could just juse the php built-in development server to run the tests.

So I ended up by adding  a service for selenium with chrome and  a service for mysql (the project I was trying this on, didn't really need other services), and I just added a `php -S` statement to the script that ran the tests, I had a running server.

I still needed a small hack, to make the browser in the selenium container actually find the webserver. I ended up using a `sed`-command on the configuration file of my test suite in codeception.

For reference, here is a part of my `gitlab-ci.yml` file:

```
    - LOCAL_IP=$(ip route get 8.8.8.8 | awk -F"src " 'NR==1{split($2,a," ");print a[1]}')
    - echo Local IP $LOCAL_IP
    # Run the php built in server. Don't try to run nginx as a service, that only causes troubles. ;-)
    - php -S 0.0.0.0:8080 -t public/ 2> /dev/null &
    - sed -i "s/localhost:8080/$LOCAL_IP:8080/" tests/acceptance.suite.yml
```

Note that for this to work, I had to fiddle with the php container to have iproute2 installed.

If you want to see how it actutally works; I created a [small demo project on gitlab](https://gitlab.com/johanv/codeception-ci-demo/pipelines) that uses codeception in gitlab-ci to run a couple of browser tests. You can find the source code on gitlab: [https://gitlab.com/johanv/codeception-ci-demo](https://gitlab.com/johanv/codeception-ci-demo).

