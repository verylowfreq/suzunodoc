`docker pull jekyll/minimal`
`docker run -it --rm --volume="%CD%:/srv/jekyll:Z" jekyll/minimal jekyll build`
`docker run -it --rm --volume="%CD%:/srv/jekyll:Z" -p 4000:8082 jekyll/minimal jekyll serve`
