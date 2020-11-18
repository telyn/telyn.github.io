---
title: "The Big String Freeze, Pt. 1"
publish: true
---

At my place of work our primary project is a medium-large Ruby on Rails project,
with about 1700 ruby files totalling about 90k lines of code. It’s a bit of a
legacy project, having been originally written in Ruby 1.8 [^rubyversion] for
Rails 2.

It's now much more modern, of course, but one thing that hasn’t ever been done
is a big sweep of the codebase to switch to immutable strings.

That on its own is pretty easy to accomplish! I ran the following command to add
the `# frozen_string_literal: true` at the top of each file, with an empty line
underneath it.

```bash
rubocop -A --only 'Style/FrozenStringLiteralComment,Layout/EmptyLineAfterMagicComment'
```


Job done! `git commit -a` time, right? Well, I’ll just run the tests first.

![Lots of tests failed.](/assets/img/big-string-freeze-failures.png)

Okay, I guess we must’ve been using some mutable strings. But which
ones? With 1700 files finding out is a daunting task. But it’s in these daunting
tasks I find the most joy as a software developer. So, let's carry on.

What I want to know ideally is precisely which strings we need to make mutable
in order to get the tests to pass. With a fairly comprehensive test suite
already in place I’m in a good position to find out. The problem is that the
errors may not present themselves quite as simply as a message spat out by the
test suite saying ‘this string cannot be mutated on this line’. Due to our code
having a fair whack of dodgy error handling we may be swallowing that
error and just calling `return nil`, or something along those lines.

Given that, it’s best we find out which file the immutable string is in, at
least. That combined with the stack trace from the failing test ought to help
pin it down. But how can we determine which change caused the tests to start
failing?

Well, if we make one commit per file, the answer is easy - `git bisect`. And
we can make one commit per file pretty easily with a little shell magic:[^pragprog]

```bash
#!/bin/bash

rubocop -A --only 'Layout/EmptyLineAfterMagicComment'
git add .
git commit -m "Ensure empty lines exist after all magic comments"

rubocop -A --only 'Style/FrozenStringLiteralComment,Layout/EmptyLineAfterMagicComment'

# Make a list of each file that was modified
# I put it in log/stringfreeze.log because I
# have a .gitignore pattern for log/*.log
git status | \
        grep -A 9000 '^Changes not staged' | \
	grep -E '^[[:space:]]*modified:' | \
	sed -e 's/^.modified: *//' > log/stringfreeze.log

cat log/stringfreeze.log | while read file; do
  git add "$file"
  git commit -m "Add frozen_string_literal to $file"
end
```

Now I've set up my 1700 commits and checked the tests fail, I can run
`git bisect` and pinpoint what's causing test failures.

In the second and final part of this post I'll go into detail on the
`git bisect` part of the process and get all the tests passing again.

[^rubyversion]: At least, I *think* it was written for Ruby 1.8 - the git
                history only goes back to the date of the migration from
                subversion.

[^pragprog]: The night before I edited this post I was reading The Pragmatic
             Programmer. I can check **Tip 26 - Use the Power of Command Shells**
             off my list 😜
