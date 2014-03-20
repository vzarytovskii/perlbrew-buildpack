Perlbrew buildpack
=======

Deploy Perl applications to [Dokku](https://github.com/progrium/dokku/) in seconds.

Tested only with Dokku's [buildstep](https://github.com/progrium/buildstep/)

## Step 1

Write simple app, save it as myapp.pl:

```perl
#!/usr/bin/env perl
use Mojolicious::Lite;

get '/' => sub {
  my $self = shift;
  $self->render('index');
};

app->start;
__DATA__

@@ index.html.ep
% layout 'default';
% title 'Welcome';
Welcome to the Mojolicious real-time web framework!

@@ layouts/default.html.ep
<!DOCTYPE html>
<html>
  <head><title><%= title %></title></head>
  <body><%= content %></body>
</html>
```

## Step 2

Create a 
[cpanfile](http://search.cpan.org/~miyagawa/Module-CPANfile-1.0002/lib/cpanfile.pod)
that lists dependencies:

```
requires 'Mojolicious', '2.0';
```

## Step 3
Create file ```.env``` with contents:
```sh
BUILDPACK_URL=https://github.com/vzaritovsky/perlbrew-buildpack.git
```

## Step 4

Create file called ```.perl_version``` with contents:

```sh
5.16.1
```
where ```5.16.1``` is required perl version

## Step 5

Create file called ```Procfile``` with contents:
```sh
web: perl myapp.pl daemon -l http://*:$PORT
```

## Step 6

Deploy using [dokku](https://github.com/progrium/dokku/)

Watch:

```
Counting objects: 5, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 342 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
-----> Building perltest ...
       Fetching custom buildpack
Cloning into '/build/buildpacks/custom'...
       Perl app detected
-----> Found perlbrew installation at /build/app/local/perlbrew
-----> Required perl: 5.16.1
-----> Found required perl: 5.16.1
-----> Bootstrapping cpanm
       App::cpanminus is up to date. (1.7001)
       local::lib is up to date. (2.000004)
-----> Installing dependencies
-----> Discovering process types
       Procfile declares types -> web
-----> Releasing perltest ...
-----> Deploying perltest ...
-----> Cleaning up ...
=====> Application deployed:
       http://perltest.yourhost.tld

To dokku@yourhost.tld:perltest
   36b6b36..d990a8f  master -> master
```

## Step 7
Open http://perltest.yourhost.tld in your web browser.
