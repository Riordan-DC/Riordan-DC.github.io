Readme
------

command: to serve locally

bundle exec jekyll serve


### Issues:

* When you include an image in a post remember to respect captialisation. Local jekyll site was fine with me using .png when the image in fact had the extension .PNG. On github pages the images didnt show up. I found out this issue by downloading the site build artifacts and checking my images were indeed available, just not when referenced using the lower case extension.
* You can ask github pages to select which branch to build your site from. When a commit is pushed to the branch the site is rebuilt and deployed. In the Actions tab of the github repo page you can observe the build process and download the build artifacts.
