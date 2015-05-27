# rubocop_workflow
Note: The rest of this document assumes you are using the Bash aliases found in rubocop.aliases.
```
  $ source rubocop.aliases
```
Using Rubocop (Ruby Cop) with testing and git.
# Prelude
A place to document what I use to clean up mt Ruby code. I use 3 tools in this : 
git and git flow to have confidence in what I mangle.
MiniTest::Spec to make sure things still work. Although it is possible to ruse Rubocop on a Brown field project. Just igmore the rake calls in this document.
Rubocop to check that things adhere to a Ruby Style Sheet. (Ref: 1), (Ref: 2).


# Preflight Checklist:
1. Vreate a new feature branch for this.
```
  $ git flow feature start rubocop.tidy
```


2. Installation
Install Rubocop on your system.
```
  $ sudo gem install rubocop
```

Or it you have a Gemfile (recommended):
```
  # add to your Gemfile:
gem 'rubocop', require: false

  # then:
  $ bundle
```

3. Run your tests furst.
```
  $ cd ./specs
  $ rake
```

4. Initial  rubocop run.```
  $ rubo.simple
```
This should produce a lot of output with a summary like:
```
.
C: 83:  8: Align the parameters of a method call if they span more than one line.
== lib/tasks/config.rb ==
C:  4:  1: Missing top-level class documentation comment.

34 files inspected, 590 offenses detected
```
DO NOT PANIC!
This is probably expected. In the output above you should see lines beginning with 
'C' - Style Guide exceptions.
'W' -  Lint-kike warnings
'E' - Errors (syntax?).
the vast bulk of these will probably be 'C' exceptions.

5. check ot see if any files can be excluded from rubocop.
Sometimes you may source from another project embedded within your source tree. Obeying the NTOPC rule (Never Test Other People's Code): You can exclude these files or directories in the Rubocop configuration file.
```
  # in ./.rubocop.yml
AllCops:
  Exclude:
    - 'build/pdf*/**'
    - 'templates/**'
```
Now repeat step 4. At least now you are only seeing legitiment warnings from your code.
6. Get a summary of where your offenses are of what type.
```



 

