# Ubuntu in Heroku

▶️🐳 This is a guide on how to use Ubuntu on Heroku.

> *Note that this guide does not use pure Ubuntu from docker rather uses a ubuntu based on OS made by Heroku itself*

## Usage

Here's the steps to use Ubuntu on Heroku:

1. Loging into Heroku Login:
```bash
$ heroku login -i
heroku: Enter your login credentials
Email: me@example.com
Password: ***************
Two-factor code: ********
Logged in as me@heroku.com
```
2. Loging into Heroku containers:
```bash
$ heroku container:login

Login Succeeded
```
3. Creating a Heroku Create: *(Not requied if you aleardy have a app)*
```bash
$ heroku create
Creating safe-journey-10471... done, stack is heroku-18
https://safe-journey-10471.herokuapp.com/ | https://git.heroku.com/safe-journey-10471.git
```
4. Pushing the Heroku App:
```bash
$ heroku container:push worker -a safe-journey-10471
=== Building worker ...
...
...
Your image has been successfully pushed. You can now release it with the 'container:release' command.
```
> *Note: here we are using worker and not web process id because web require a listening port, we are also using the app from berfore with the `-a` parameter. You should use your app name created*
5. Releaseing the Heroku App:
```bash
$ heroku container:release worker -a safe-journey-10471
Releasing images worker to safe-journey-10471... done
```
6. Scaling your app to work as a worker
```bash
$ heroku ps:scale worker=1 
Scaling dynos... done, now running worker at 1:Free
``` 
7. Checking if the `worker` in working:
```bash
$ heroku ps -a safe-journey-10471
Free dyno hours quota remaining this month: 549h 52m (99%)
Free dyno usage for this app: 0h 0m (0%)
...
...
=== worker (Free): /bin/sh -c bash\ heroku-exec.sh (1)
worker.1: up 2020/01/19 04:35:29 -0800 (~ 1m ago)
```
7. Shell into your app!
```bash
$ heroku ps:exec -a safe-journey-10471 -d worker.1
Running this command for the first time requires a dyno restart.
Do you want to continue? [y/n]: y
Initializing feature... done
Restarting dynos... done
Waiting for worker.1 to start... done
Establishing credentials... done
Connecting to worker.1 on ⬢ safe-journey-10471...
/ $ uname -a
Linux c3ce7420-4538-446e-912f-67168232773f 4.4.0-1057-aws #61+hf245703v20191104b1-Ubuntu SMP Mon Nov 4 15:32:25 UTC 2019
 x86_64 x86_64 x86_64 GNU/Linux
/ $
```
> *Note: `-d` parameter has to be put after `-a` parameter or the `-d` parameter will not be detected.* 

And there you go, now you have your own Heroku container running Ubuntu!
