<!--
.. title: gitlab-ci and yarn
.. slug: gitlab-ci-and-yarn
.. date: 2019-06-29 21:02:12 UTC+02:00
.. tags: tests,ci,frontend,yarn,docker
.. category: 
.. link: 
.. description: 
.. type: text
-->

For my previous post, on [gitlab-ci and codeception](/posts/gitlab-ci-codeception-and-selenium-web-tests), I created a small project on gitlab that [runs selenium webtests during gitlab-ci](https://gitlab.com/johanv/codeception-ci-demo).  The project being tested is just a small vue.js-application.

Recently I rewrote that small application; now it uses [a single file vue component](https://vuejs.org/v2/guide/single-file-components.html). I'm not completely sure what I've done, but it seems to work, and it involves [webpack encore](https://github.com/symfony/webpack-encore) and [yarn](https://yarnpkg.com/en/).

![Running yarn from gitlab-ci](/galleries/misc/gitlab-codeception-yarn.png)

Now the challenge was to run yarn from gitlab-ci. I was looking for a docker container with yarn (even tried to build one myself), but it took me some time before I discovered that yarn is included in the [node](https://hub.docker.com/_/node)-container.

So I now use this node container in an extra task of the build stage in my [.gitlab-ci.yaml](https://gitlab.com/johanv/codeception-ci-demo/blob/master/.gitlab-ci.yml#L25). I create an artifact with the generated javascript, which is used in the test stage. [That seems to work](https://gitlab.com/johanv/codeception-ci-demo/pipelines/68644877).

As I said before, I am new to vue, and it's the first time I use yarn and webpack. So maybe I am making some obvious mistakes. Please let me know if there are things that can be done in a better way.
