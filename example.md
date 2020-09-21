# How to use easysite to create a website on Github

It's assumed you have basic knowledge of Github.

- Create a new repo and make it public.
- Turn on Github Pages for your repo. Go to "Settings", then "Options", scroll to "GitHub Pages", and set the branch to master, and the folder to "/docs".

You now have a website working.

- Add `index.html` from this repo to the `docs/` folder of your repo.
- Add markdown files at the top level. You can do that by clicking the "Add file" button on the code page. I recommend creating a markdown file on your local computer and then pasting into the editor. However, you can type into the editor directly if you want. That is in fact how I'm creating this page. If you want to create a site on an iPad, for instance, that is your only option. Be sure to set "Soft wrap" for the Line Wrap Mode or it will not be fun to type into the editor.
- View your pages at `https://username.github.io/reponame/?file` where `username` is your Github username and `file` is the name (without the .md extension) of your markdown file.
- You probably want to create an index page. If you call it `index.md`, it will be rendered when you go to `https://username.github.io`. You can link to other pages with the markdown [page description](?file). Yes, you really a question mark followed by letters really is a valid way to refer to a URL in markdown.

# How it works

Note the `?` in the URL. Although that might seem strange, it is the secret sauce that makes this work. Here's what happens with `https://username.github.io/reponame/?file`. The `?file` is a parameter named "file" that was not given a value. Javascript is used to parse out the name of the first parameter, which just happens to be the name of the file without the `.md` extension. The `/?` means no page name was provided, in which case the server will automatically substitute `index.html` for the name of the page. So it calls `index.html` (which you added from the easysite repo), and the first parameter is passed to `index.html`. Now that it knows the filename, it will either render `index.md` if no filename was provided, or downloads the associated markdown file from your repo, converts it to html, and adds to the page.

tldr; I'm using some features of URLs in non-standard ways plus the ability of Javascript to download the content of a file from a publicly available Github repo.

# Customization

One of the goals with easysite was to create something that was extremely easy to customize. You can add any CSS you want inside `index.html`. Just add it below the existing CSS but before `</style>`. I've provided some basic styling, but you may want something else, and that's a good thing. I'm using markdown-it to convert to html, plus the mathjax plugin to support equations. You can easily change that if you want to use a different approach or add other plugins.

It's interesting to note that there is no particular reason you need to create your files in markdown format. You could use org-mode, asciidoc, or any other format, so long as you have a way to render it with Javascript. You could even post in pure html. That would be helpful if you enjoy writing html (it's possible that someone enjoys html) or if you want to do the conversion locally because you don't have an online Javascript converter. You'd need to change the line that does the downloading to use the right extension:

```
var filename = "https://raw.githubusercontent.com/" + url.host.split(".")[0] + url.pathname + "master/" + fn + ".md";
```
