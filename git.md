- `git cat-file -p HASH_VALUE`
- `git hash-object -w FILE`
- `git update-index --add --cachedinfo FILE_ENTRY_IN_DB`
- `git write-tree TREE_HASH_VALUE`
- `git read-tree TREE_HASH_VALUE`
- `echo "COMMIT COMMENT" | git commit-tree TREE_HASH_VALUE`
    - tree in index area --> commit --> tree1 --> commit --> tree2 --> commit --> tree3
- `git log --stat TREE_HASH_VALUE`


# How Git stores objects?

```js
//  for example:

content= "Hello World!"


//  1. construct header

header = "blob #{content.bytesize}\0" // header = "FILE_TYPE #{content.bytesize}\0"
// output: "blob 12\u0000Hello World!"


// 2. concatenate the header and the original content
// 3. calculate the SHA-1 checksum

store = header+content
sha = sha1(store)
// output: "bd9dbf5aae1a3862dd1526723246b20206e5fc37"


// 4. compress content

deflatedContent = zlip.createDeflate(store)


// 5. save to filesystem
...
```
---

# Tips to contributing to work

1. use `git diff --check` to check potential error 
2. make each commit a logically separate changeset
    - use "staging" to split a massive changeset into small ones
    - or `git add --patch` to stage part of the changes in one file
    - meaningful commit message

> ## Commit message format ##
> Capitalized, short (50 chars or less) summary
> 
> More detailed explanatory text, if necessary.  Wrap it to about 72
> characters or so.  In some contexts, the first line is treated as the
> subject of an email and the rest of the text as the body.  The blank
> line separating the summary from the body is critical (unless you omit
> the body entirely); tools like rebase will confuse you if you run the
> two together.
> 
> Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
> or "Fixes bug."  This convention matches up with commit messages generated
> by commands like git merge and git revert.
> 
> Further paragraphs come after blank lines.
> 
> - Bullet points are okay, too
> 
> - Typically a hyphen or asterisk is used for the bullet, followed by a
>   single space, with blank lines in between, but conventions vary here
> 
> - Use a hanging indent

3. 


