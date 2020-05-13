# nice tools

## links to overviews

- [overview of tools/services for web development](https://dev.to/davidepacilio/50-free-tools-and-resources-to-create-awesome-user-interfaces-1c1b)

## screen recorder

To create documentation for an ui or demonstrate bugs.

Windows: [Screen to gif](https://www.screentogif.com/)
Linux: 
- Deepin screen recorder
- [Peek](https://github.com/phw/peek)

## terminal recorder

[ascii cinema](https://asciinema.org/) save terminal session in a cast file which can be played in a terminal or on the website. The cast file is a text file, but it can not be used to be directly copy pasted into a terminal. The asciidoc player is needed.

usage:

```
## install 
sudo apt-get install asciinema

## record and save in file
asciinema rec ./file.cast
# exit via crlt + D

## replay a record
asciinema play ./file.cast

## dumps the content of a record on console
asciinema cat ./file.cast

## record and save to web ( you need account to view it)
asciinema rec
# exit via crlt + D
# press enter
```

