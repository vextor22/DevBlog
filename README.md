# DevBlog

The hosting for my personal blog has been moved from a Digital Ocean droplet running Ghost CMS to Github Pages using Hugo.

The driving factor is cost ($5 vs free), with a secondary consideration for maintenance. It isn't any fun periodically 
revisiting the deployment of my blog to figure out why the Let'sEncrypt client has stopped refreshing, or how to update
through a major version. 

With GitHub Pages, the hosting for my entirely static content blog is now free, faster, and more secure. It is also simpler
to create a new post, new content is always backed up by virtue of Git, and hosting updates will largely be handled by 
the managed service.

# Building the site

To build the site, simply run `hugo` in the DevBlog directory:

```
hugo
...

Start building sites … 

                   | EN  
-------------------+-----
  Pages            | 26  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     | 11  
  Processed images |  0  
  Aliases          |  0  
  Sitemaps         |  1  
  Cleaned          |  0  
```

# Run test server

Simply run `hugo serve` in the DevBlog director.

# Automated building

On push, Github Actions builds the site and deploys it to the gh-pages branch, which is accessible under [www.matthiggins.dev](www.matthiggins.dev)