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
  $ git flow feature start rubocop.tidyup
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
  $ rubo.offenses


19  Style/Documentation
12  Metrics/LineLength
2   Style/AlignParameters
1   Metrics/MethodLength
--

34  Total
```
Now if you see any lint warnings, then attact them first.




```



 

7. Run Auto Correct. 
This will eliminate most of your headaches. Be aware that the Ruby Style guide Rubocop is using is __very__ opinionated.
You may not kike all of its decisions.
```
  $ rubo.correct
  $ git commit -am 'after rubocop -a. autocorrected stuff.'
```
Note: We commit the results on our feature branch so we can revert or diff as needed. Now rerun your tests.
```
  $ (cd ./specs; rake)
```

8. Run the lint rubocop flag.
```
  $ rubo.lint
```
I usually save this to file, which I consult as I fix things.
9. Create a TODO list for correcting things.
```
  $ rubo.todo.gen
```
This will create a file : ./.rubocop_todo.yml 
It will have all the cops turned off that gave you offenses. Next, you will have to inherit 
the main Rubocop yaml file, if you needed to exclude some files in Step  5.
```
  $ rubo.todo.edit
  # Add the following:

inherit_from:
  - ./.rubocop.yml
```
10. Run Rubocop with no offenses to be found.
```
  $ rubo.todo.cop
  # You should see:

Inspecting 34 files
..................................

34 files inspected, no offenses detected
```
# Main Workflo
1. Edit TODO list.
```
  $ rubo.todo.edit
```
Turn on one tyoe of checking by setting the cop to 'Enabled: true'
2. Re-Run the TODO cops.
```
  $ rubo.todo.cop
```
Examine the output carefully. If you need to, check the style guide for help. (Ref. 2)
3. Edit your source.
```
  # One way to do this:
  $ vi `rubo.todo.cop.files`
  # Edit files as you go and save
o
4. Rerun your tests
````
  $ (cd ./specs; rake)
```
5. Checkpoint with git
```
  git -am 'Cleaned up Rubocop offenses'
```

Return to Step 1.
# End of workflow
Some final thoughts.
When you have cleanedup all offenses, and committed to git, you may have seen some refactoring steps you could also do. Rubocop forces you to think ways to removed the offending subsets of code by reorganizing or refactoring. Remember refactoring
should not change or add any ofyour test code. After each refactor process, rerun your specs and rerun rubocop.
```
  $ (cd ./specs; rake)
  # fix, then
  $ rubocop
```
Note: The last step above is just running Rubocop with the usual config yaml file.
# Automating Cops
Once you have a clean code base, you may want to merge that feature branch into develop branch. 
Then you may wnat to add Rubocop to your specs Rakefile. See Ref 1. on how Rubocop integrates with Rake.
```
  $ cd ./specs
  $ rake cops
  $ Consider making that task depend on the 'test' task.
```
# Brown Field Projects
If you have a legacy project with no or incomplete tests, you won't have the safety net of running your specs. You might consider adding some tests and test 
```
  $ git flow feature finish rubocop.tidyup
```
doubles before you start on a Rubocop adventure. However, if reading the code is a
chore, then use Rubocop to at least get the code some nicer state before adding a lot of tests.
Note: See my repo: filemockery_minitest for some useful hints and 
supporting code for File mocks and stubs.
# References
1. Rubocop :https://github.com/bbatsov/rubocop/blob/master/README.md#installation
2. Ruby Style Guide used with Rubocop  :https://github.com/bbatsov/ruby-style-guide



