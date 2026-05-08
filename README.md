XTMax-only fork of [Ted Fried's MicroCore Labs Projects repo](https://github.com/MicroCoreLabs/Projects)

The original repo is huge, with many unrelated projects. Since I wanted to just
work on XTMax portion and not have to worry about whatever made the repo so
huge, I made this.

Process
=======
1. I forked the original repo on Github
2. I cloned it with filters when cloning to my laptop
   `git clone --filter=blob:none --sparse <this repo>
3. Change directory to the new clone.
   `cd XTMax`
4. Materialize only the files I wanted (the files in the `XTMax` directory).
   `git sparse-checkout set XTMax`
5. I made a new directory with a new bare repo
   `cd ../`
   `mkdir XTMax-split`
   `cd XTMax-split`
   `git init --bare`
6. split the subtree out back in the original
   `cd ../XTMax-sparse-checkout`
   `git subtree split --prefix=XTMax --annotate="(split)" -b split`
7. push it to the new repo
   `git push ../XTMax-split split:main`
8. make the bare repo not bare by changing `core.bare=true` to `false`
   `cd ../XTMax-split`
   `git config --file .git/config core.bare false`
9. Push to github
   `git remote add origin <another repo>
   `git push origin main`

Or at least, it was _something_ like that. I have to admit it was wierd git
trickery and I'm still not sure I achieved what I wanted: original history
preserved, no giant repo of random files, and the ability to push changes back
to MicroCoreLabs' repo if that's ever needed.



