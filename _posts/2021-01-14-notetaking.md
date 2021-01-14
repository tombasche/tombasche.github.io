---
layout: archive
title:  "How I keep notes"
---

Towards the end of last year (2020) I decided on an _actual_ New Years Resolution. I was going to start taking daily notes, keep them somewhere backed-up and automate the process (more or less). I figured this would help me look back at the year and identify the things that I learnt, the things that caused me pain and the things I worried about (that probably seem trifling in hindsight!). As well as this it would include day-to-day tasks and checklists.

I'd tried this before - the diary thing - and after keeping piecemeal notes in various forms[^1] I decided I'd need one solution to rule them all.

## Here were my requirements:

  * Be able to mark up notes with headings, tables, dot points, italics etc. Nothing like some good ol' _emphasis_ where necessary. 
  * Be able to render all of them into one PDF if needed. 
  * Shareable across time and space ... not necessarily some sort of real time 'syncing' but more or less available on multiple machines.
  * Searchable. Would be nice to know all the times I wrote down 'crap' or something and graph it over time. [^2]
  * Minimal start-up time. Can start writing after a single command. The easier it would be the more I would take up a diary habit. 

## A few suggestions from trawling the internet which I didn't end up going with:

  * [TiddlyWiki](https://tiddlywiki.com/). I heard this from a Joe Armstrong talk. Looks like a good idea, but probably too heavy-handed for what I needed. Also the Chrome plugin I tried didn't work, which wasn't a great start.
  * [jrnl](https://jrnl.sh/en/stable/). Looks bloody cool, but probably a bit _too_ commandliney. I liked the idea of a markdown file which I could render.
  * emacs-orgmode. I could probably invest the time to learning emacs, but fear I'll get stuck one day and have to mash Ctrl-everything until it all goes away.
  * Dropbox. I could probably have just synced up a directory to Dropbox, but I liked the idea of being able to read them (rendered!) in the browser if I could. Which led to ... 


## The solution!

* Markdown. I write this blog in Markdown, so it made sense to stick to one 'language'.
* Private Github repository. Github renders markdown and is free. Probably also not going anywhere any time soon. It also means I can just push up changes on one machine, and pull them down on another. Github actions might allow me to automate the PDF rendering and push it somewhere else?
* Search on Github is pretty reasonable, although I'm more likely to just use the search in VS Code or `grep` if I'm desperate. 
* A script to make it easy ... this was possibly the funnest part![^3] Here is my `./new.sh` to create a new file: 

```bash
#!/bin/zsh

go run src/main.go
```

Okay, ha ha. Here's the guts of it:

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
	"os/exec"
	"path/filepath"
	"strconv"
	"time"
)

type mkdir func(path string, mode os.FileMode) error

func main() {
	t := time.Now()
	year, month, day := t.Date()
	fmt.Printf("Welcome to %v %v, %v!\n\n", day, month, year)
	dir, err := createFolderDirectory(os.MkdirAll, year, month)
	if err != nil {
		handleError(err, "Error creating directories: %v\n")
	}

	h := headingText(t)
	filename, err := createTodaysFile(dir, formattedDay(day, t.Format("Monday")), h)
	if err != nil {
		handleError(err, "Error creating todays file: %v\n")
	}
	err = openEditor(filename)
	if err != nil {
		handleError(err, "Error opening the editor with file %v: %v\n")
	}
}

func createFolderDirectory(mkdirFunc mkdir, year int, month time.Month) (string, error) {

	yearStr := strconv.Itoa(year)
	path := filepath.Join("notes", yearStr, month.String())
	err := mkdirFunc(path, os.ModePerm)
	if err != nil {
		return "", err
	}
	return path, nil
}

func createTodaysFile(dir string, name string, h string) (string, error) {
	filename := fmt.Sprintf("%v.md", name)
	path := filepath.Join(dir, filename)

	if fileExists(path) {
		fmt.Println("This file already exists!")
		return path, nil
	}

	file, err := os.OpenFile(path, os.O_RDONLY|os.O_CREATE, 0644)
	if err != nil {
		return "", err
	}
	heading := []byte(fmt.Sprintf("# %v\n\n", h))
	err = ioutil.WriteFile(path, heading, 0644)
	if err != nil {
		return "", err
	}
	return path, file.Close()
}

func formattedDay(day int, dayName string) string {
	return fmt.Sprintf("%v-%v", day, dayName)
}

func headingText(t time.Time) string {
	return t.Format("Monday, 2 January 2006")
}

func openEditor(loc string) error {
	fmt.Printf("Opening %v in VS Code\n", loc)
	cmd := exec.Command("code", loc)
	err := cmd.Run()
	if err != nil {
		return err
	}
	return nil
}

func handleError(e error, msg string) {
	fmt.Fprintf(os.Stderr, msg, e)
	os.Exit(1)
}

func fileExists(f string) bool {
	info, err := os.Stat(f)
	if os.IsNotExist(err) {
		return false
	}
	return !info.IsDir()
}
```

It's probably not the _best_ solution, but as they say: "perfect is the enemy of done". Each morning, I run this script which: 
  1. Creates a file in the appropriate directory, determined by the date I run it e.g. `2021/January/1-Friday.md`.
  2. Inserts a heading based on the date. `# Friday, 1 January 2021` and some newlines to get me started.
  3. Opens it in VS Code. 

I kept config to a minimum here as I was going for speed rather than options. I think the whole thing took me less than an hour to cobble together once I knew what I wanted. It's been two weeks and I've been pretty happy with it so far.[^4]

Here's how files are saved with a `./save.sh`:

```zsh
#!/bin/zsh
git add notes
git commit -m "Saved entry $(date +'%m-%d-%y')"
git push
```

I though about incorporating a file watcher to just `git commit` each time I save, and periodically push it back up to Github, but I think it would bother me if I wasn't in control of it, and also it might spam Github a bit ... 

As for generating a PDF? I haven't really wanted to do this yet, but I have played around a bit with `pandoc` to get this beauty - a monthly digest, just a single line on the terminal[^5]:

```zsh
for f in notes/2020/December/**.md; pandoc $f -o december.pdf
```

It looks quite beautiful actually! [Here's an example from their docs](https://pandoc.org/demo/example13.pdf)

I think my next step would be incorporating the digest into the workflow, possibly a weekly one generated on a Friday. I wonder if I could even only render stuff I think is really important for the digest (a word cloud maybe?). EPUB and sync it to my Kindle?

## Lessons learned

* There's a _lot_ of options out there for personal note-taking.
* Committing to a single one to use daily is probably most important, and one that meets your needs! 
* Go's date format string is an excellent idea, although having to remember the reference date, perhaps not so much.


### Footnotes

[^1]: Trello, Google Keep, Apple Notes, markdown files in `~/Documents`, messages to myself on Slack, scribbles in a physical notebook, comments in Jira, the list goes on ... 

[^2]: Sentiment analysis? How far can I take this?

[^3]: Perhaps this was all just a thinly-veiled excuse to write _more_ code.

[^4]: This has given me a new appreciation for 'maximising the amount of work not done'. 

[^5]: ...after installing a PDF renderer for OS X