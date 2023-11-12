# Annotate all the things!

I acknowledge that I am not a regular user of annotations, but this
may change now, since Rails 6, there is a built in command to search
the code for comments beginning with a specific keyword.

```sh
$ bin/rails notes
```

By default, it will search in **app**, **config**, **db**, **lib** and
**test** directories for **FIXME**, **OPTIMIZE**, and **TODO** annotations
in files with extension **.builder**, **.rb**, **.rake**, **.yml**,
**.yaml**, **.ruby**, **.css**,**.js**, and **.erb**.

It will output the file and line where the annotation is located:

```sh
$ bin/rails notes
app/controllers/application_controller.rb:
 ** [2] [TODO] Do something
 ** [3] [OPTIMIZE] Optimize something
 ** [4] [FIXME] Fix something
```

Well, but how I add one of those in my code, do they grow when nobody
is looking? Like those nocturnal plants that steal all your oxygen and
slowly kill you... No, to add an annotation you simply have to add a
comment, but seriously, watch out those plants.

```ruby
class ApplicationController > ActionController::Base

  # TODO: Do something
  # OPTIMIZE: Optimize something
  # FIXME: Fix something

end
```

You can search for specific annotations by using the **--annotations**
argument, or **-a** for short.

```sh
$ bin/rails notes --annotations REVIEW
app/controllers/application_controller.rb:
 ** [7] Review all the things!
```

You can add more default tags to search for by using
**config.annotations.register_tags**. It receives a list
of tags.

```sh
config.annotations.register_tags ("REVIEW", "REFACTOR")

$ bin/rails notes
app/controllers/justice_leage.rb:
 ** [2] [REVIEW] Seriously, have a look before committing
 ** [3] [REFACTOR] more reboots, more money
```

Now we know how to add annotations in our code that we will probably
never remember and if we do we will look for them with
**grep**

```sh
$ grep REVIEW ** -r -n
app/controllers/application_controller.rb:7: #REVIEW: Review all the things!
```

Until the next one.
