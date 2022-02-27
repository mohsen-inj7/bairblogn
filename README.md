# BAIR Blog Instructions 

This is the repository for the BAIR blog, located at `bair.berkeley.edu/blog/`.
Posts are located in `_posts`. Preview locally, and then push to the master
branch, which will generate a preview at `bairblog.github.io`. This is the link
we give to authors to preview their posts.



**TODO: transfer some stuff later in the README here ...**

# Writing Posts in Markdown

There are a few important things to know about our specific blog format.

## Images

To avoid putting all images and gifs into this GitHub repository, and to avoid
having to copy the entire `_site` folder to where the blog lives, we save the
images inside the folder `/project/eecs/interact/www-bair/static/blog`. Each
blog post gets its own folder of images/gifs, even if it has only one.

Make sure you avoid using the `site.url` and `site.baseurl` liquid tags. That
is, earlier we used the following code:

```
<p style="text-align:center;">
<img 
src="{{site.url}}{{site.baseurl}}/assets/mh_test/different_tests.png" 
alt="different_tests" width="600"><br>
<i>
Functions $f$ and $g$ can serve as acceptance tests for Metropolis-Hastings.
Given current sample $\theta$ and proposed sample $\theta'$, the vertical axis
represents the probability of accepting $\theta'$.
</i>
</p>
```

But **do not use that**. Instead, use an explicit link to the static folder:

```
<p style="text-align:center;">
<img 
src="http://bair.berkeley.edu/static/blog/mh_test/different_tests.png" 
alt="different_tests" width="600"><br>
<i>
Functions $f$ and $g$ can serve as acceptance tests for Metropolis-Hastings.
Given current sample $\theta$ and proposed sample $\theta'$, the vertical axis
represents the probability of accepting $\theta'$.
</i>
</p>
```

The above also represents how we prefer to use captions for figures, and how to
center images, control width, etc. Note, however, that using hyperlinks in
Jekyll format (using `[text](link)`) for the captions doesn't work, so use the
explicit HTML code.


# How to Update


Update notes:

- We now have two branches, master and production. Master is used for
  `bairblog.github.io` which is GitHub's built-in feature for updating websites.
  The production branch is used for copying the files over to the place where
  the blog lives.
- The production branch has specific `baseurls` and `urls` that shouldn't
  change.
- Watch out for setting comments to be False in the master branch, and for
  making posts invisible until they're ready to be published.
- Once things have been copied over, you're not done! Change permissions to give
  everyone in the `interact` group writing permissions. See the bottom of this
  RAEDME.
- TODO: Fix README. Yeah, I know ... maybe during winter break!

---



The easiest way to get started is to copy one of the older posts we have and
start from there. Please make the title informative and also to start with the
date first and then the title.  You can also start from another Markdown
"starter file" and the editors here can fix it.

We strongly recommend you preview locally. To do so, run `bundle exec jekyll
serve` as described in [the Jekyll starter guide][2]. If all goes well, and you
[have jekyll installed][7], you should see the following output:

```
danielseita$ bundle exec jekyll serve
Configuration file: /Users/danielseita/bairblog/_config.yml
Configuration file: /Users/danielseita/bairblog/_config.yml
            Source: /Users/danielseita/bairblog
       Destination: /Users/danielseita/bairblog/_site
 Incremental build: disabled. Enable with --incremental
     Generating... 
                    done in 0.434 seconds.
 Auto-regeneration: enabled for '/Users/danielseita/bairblog'
Configuration file: /Users/danielseita/bairblog/_config.yml
    Server address: http://127.0.0.1:4000/blog/
  Server running... press ctrl-c to stop.
```

Note that this may not work for outdated Jekyll versions. I'm using version
3.4.4. here.

Once you did that, copy the URL `http://127.0.0.1:4000/blog/` to your web
browser and you should see your post there. Double check that all links are
working and that all images are showing up. Oh, and if you modify your post and
save it, Jekyll automatically refreshes the website, so just refresh it in your
browser.

Tips:

- Make sure that `visible: True` is set in the header when previewing.

- When hyperlinking to other posts that have **not yet been released**, (e.g. if
  your post builds upon an earlier one) please use the `site.baseurl` feature.
  See Sergey's welcome post for an example of how to do this. You can do it
  with:
  
  ```
  [1]:{{ site.baseurl }}/2017/06/20/learning-to-reason-with-neural-module-networks/
  ```

  This is useful for when we adjust `site.baseurl` during local tests, so that
  links automatically work. Then in the main text of your blog, do something
  like:

  ```
  Hi everyone! In [this earlier post][1], we discussed how to reason.
  ```

  The text here will automatically form a hyperlink to the URL described with
  `[1]`. If you're linking a post which already has been released, you can just
  use the full URL since that won't change.

- For images and GIFs, please store them in the `assets` folder (an online link
  also works, see Jeff's post for examples of how to do this). You can put an
  image with code similar to the following (from Jacob's post):

  ```
  <p style="text-align:center;">
  <img src="{{site.url}}{{site.baseurl}}/assets/nmns/exploded.jpg" width="400">
  </p>
  ```

  This centers the image, puts it as a fixed width (in case it's too small to be
  the full page width) and learns to prepend the site URL and then base URL.

For additional information about writing posts, [see Jekyll's instructions][1].


# For People Deploying the Blog Live

Be careful when doing this. You can get it online without it becoming the
official blog by modifying `_config.yml` so that 

`baseurl:     "/blog"`

becomes

`baseurl:     "/blog2"`

Or change the baseurl to be something else you want. This, by the way, is why we
encourage post authors to use `{{ site.baseurl }}` when hyperlinking posts,
because then changing the baseurl automatically means hyperlinks will work.
When previewing locally, I told writers to use 

```
bundle exec jekyll serve
```

to preview. However, if you want to get this actually deployed online, you need
to run it in **production mode**:

```
JEKYLL_ENV=production bundle exec jekyll serve
```

This should *still* generate an appropriate blog for you to preview *locally* at
`http://127.0.0.1:4000/blog/`. However, the difference with this command is that
it will *automatically* configure the `_site` folder to have the correct links
in it (bair.berkeley.edu and whatever your baseurl was). If you didn't add the
production environment, the `_site` folder would contain a bunch of
`http://localhost:4000` links.

Let's suppose we've set the baseurl to be `blog2` in the configuration file.
Then we can securely copy:

```
scp -r _site seita@login.eecs.berkeley.edu:/project/eecs/interact/www-bair/blog2
```

if the `blog2` folder doesn't exist. Otherwise use

```
scp -r _site/* seita@login.eecs.berkeley.edu:/project/eecs/interact/www-bair/blog2/
```

so that the contents of `_site` go in `blog2`. (If there are any permission
denied errors, then the cause is likely a group issue, which I explain later.)

Note that you'll need permissions to push to this group (it's from Anca Dragan).
Right now only Jane and I have permissions for this. At some point, though, we
should probably figure out a way to copy only the new files we need, but this
likely won't be a problem until we get maybe 40 posts.

This will generate a blog preview on an actual, live website (and not
localhost). When doing this: 

- Be *very careful* that all links are working correctly, and that their
  baseurls are correct.

- Before actual deployment, **make sure `visible: False` is set for posts
  which are not yet released**! Future posts should only be set to visible for the
  purposes of previewing them.

- Make sure permissions are set correctly, otherwise people will not be able to
  view the `.html` files, or they may see them but the output will look awful
  (think normal HTML text without the Jekyll beautification). Run `ls -lh` and
  check that files have permissions shown as `drwxrwxr-x` for directories and
  `-rwxrwxr-x` for non-directories. This can be done recursively inside a blog
  directory with `chmod -R 775 *`.

Finally, if you're satisfied with how this looks, then change the baseurl to be
`blog`, re-bundle in production mode using the same command above, and copy the
`_site` folder over to the `blog` folder. This will make the blog live in the
desired URL.


# Other Important Stuff

From Jacky:

```
a note for future testing posts - let's change "show_comments" to "false" in the
.md during development until we publish to /blog. 

Disqus is tracking the first instance when it sees the comments section being
generated, so these comment alert emails Disqus is sending redirects to posts
under /jacky or /jane
```

ALSO ... we need to ensure that permissions AND groups are set correctly. The
permissions should be set to 775, as in:

```
chmod -R 775 blog/
```

But we also need the group to be `interact`, so please check that. My default
group is `grad10` for some reason, which prevents others in the `interact` group
from copying files over.

Thus be sure to do 

```
chgrp -R interact blog/
```

TODO: does this work even if the owner (not the group!) of the `blog` directory
is not me? I *think* it recursively checks, but not sure. Just check that all
groups are `interact` and that if I do a `scp`, I should get zero permission
denied errors.

(Update: I don't think this works, but we shouldn't have to do this each time.
Just go into the directory corresponding to the post that was just published and
change the group to `interact`. Do the same thing for the `assets` folder for
that post. Those two things should be the only modifications needed.)

It's probably best to check at minimum all the new files your commit adds, to
ensure that the groups and permissions are set appropriately. You don't need to
run the command at the top (at the `blog/` level).


# TODO

- Need a better pipeline, secure copy is inefficient
- See our Google Doc

# Questions?

Ask Daniel Seita for questions about the README.

Important references for understanding Jekyll, particularly with regards to
links:

- [Understanding baseurl][4]
- [Configuring for project GitHub pages][3] (this is a "project" page because we're
  putting it on bair.berkeley.edu/blog and not in a personal github website).
- [Changing the root URL to be the correct one][5]
- [Jekyll docs on configurations][6]

[1]:https://jekyllrb.com/docs/posts/
[2]:http://jekyllrb.com/docs/quickstart/
[3]:http://downtothewire.io/2015/08/15/configuring-jekyll-for-user-and-project-github-pages/
[4]:https://byparker.com/blog/2014/clearing-up-confusion-around-baseurl/
[5]:https://github.com/jekyll/jekyll/issues/5853
[6]:https://jekyllrb.com/docs/configuration/#specifying-a-jekyll-environment-at-build-time
[7]:https://jekyllrb.com/docs/installation/
