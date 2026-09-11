# REMINDER
- nur veränderte daten werden beim 2. backup kopiert, ganz wichtig!!
- der / entscheidet darüber ob ordner oder inhalt kopiert wird

# EXAMPLES
## BASIC SYNTAX
```
rsync -av source/ backup/
```

## LOCAL > SERVER
```
rsync -av source/ user@server:/path/
```

## SERVER > LOCAL
```
rsync -av user@server:/data/ ./data/
```

## EXACT COPY
```
rsync -av --delete source/ backup/
```

# FLAGS
`-a` full backup  
`-ǹ` test run
`-e ssh` use ssh  
`-v` verbose  
`-z` compression  
`-r` recursive  
`-p` keep permissions  
`-t` keep time  
`--delete` deletes files that are not in source  
`progress` progress bar  
`dry-run` test run

# INFO
`source/`   → copies contents  
`source`    → copies folder itself












