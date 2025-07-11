```
                    /  |                /  |/  |                                
       __   ______  $$ |   __  __    __ $$ |$$ |          ______   __   __   __ 
      /  | /      \ $$ |  /  |/  |  /  |$$ |$$ | ______  /      \ /  | /  | /  |
      $$/ /$$$$$$  |$$ |_/$$/ $$ |  $$ |$$ |$$ |/      |/$$$$$$  |$$ | $$ | $$ |
      /  |$$    $$ |$$   $$<  $$ |  $$ |$$ |$$ |$$$$$$/ $$ |  $$/ $$ | $$ | $$ |
      $$ |$$$$$$$$/ $$$$$$  \ $$ \__$$ |$$ |$$ |        $$ |      $$ \_$$ \_$$ |
      $$ |$$       |$$ | $$  |$$    $$ |$$ |$$ |        $$ |      $$   $$   $$/ 
 __   $$ | $$$$$$$/ $$/   $$/  $$$$$$$ |$$/ $$/         $$/        $$$$$/$$$$/  
/  \__$$ |                    /  \__$$ |                                        
$$    $$/                     $$    $$/                                         
 $$$$$$/                       $$$$$$/                                          
 ```

## jekyll-rw - Rewrite Rule Engine

**transform text via regex patterns + lua scripts**

### install

```bash
cargo install jekyll-rw
```

### basic usage

```bash
# simple find-replace
jekyll-rw 's/foo/bar/g' file.txt

# with regex
jekyll-rw 's/\d{3}-\d{4}/REDACTED/g' sensitive.log

# process stdin
cat data.csv | jekyll-rw 's/,/	/g' > data.tsv
```

### lua scripts

```lua
-- uppercase.lua
function transform(line)
  return line:upper()
end
```

```bash
jekyll-rw --script uppercase.lua input.txt
```

### advanced patterns

```bash
# conditional replacement
jekyll-rw 's/error/ERROR/gi if line contains "critical"' log.txt

# multi-line mode
jekyll-rw --multiline 's/<div>.*?<\/div>//gs' html.txt

# capture groups
jekyll-rw 's/(\w+)@(\w+)/\2 at \1/g' emails.txt
```

### performance

processes 100k lines/sec (single core)

uses **regex-fast** engine ([regex-fast.io](https://regex-fast.io))

### batch mode

`rules.txt`:
```
s/TODO/DONE/g
s/\s+$//  # trim trailing whitespace
s/^#.*$//  # remove comments
```

apply:
```bash
jekyll-rw --batch rules.txt *.md
```

### why not sed?

sed syntax hurts my brain. lua is easier.

also works on windows without cygwin.

### limitations

- max file size 2GB
- lua timeout 5sec per line
- no lookahead/lookbehind (use perl if you need that)

MIT • solo project by someone tired of sed tutorials

# PR Update: 2025-11-01 07:41:35
