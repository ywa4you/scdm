#!/bin/bash
wget "$1" -O scdm.html -o /dev/null

TITLE=`grep "<title>Stream" scdm.html | awk -F "|" '{print $1}' | cut -d\  -f2-`
# song's attributes
NAME=`echo $TITLE | awk -F " by " '{print $1}' | sed 's/&amp;/and/g' | sed 's/&#39;//g'`
FILENAME=`echo $NAME | tr '[:upper:]' '[:lower:]' | sed 's/[^A-z0-9]/-/g' | tr -s '-'`
ARTIST=`echo $TITLE | awk -F " by " '{print $2}'`
GENRE=`grep 'genre" content=' scdm.html | awk -F '"' '{print $4}' | sed 's/&#39;//g'| sed 's/ &amp; /,/g'`
YEAR=`grep "published on" scdm.html | awk -F ">" '{print $2}' | awk -F "T" '{print $1}' | awk -F "-" '{print $1}'`

#image conversion
wget -r -nd -A jpeg,jpg,png `grep "<img src=" scdm.html | awk -F '"' '{print $2}'` -O cover.jpg -o /dev/null
convert cover.jpg cover.png

yt-dlp -x --audio-format flac --audio-quality 0 $1 -o "$FILENAME.%s"

# assigning attributes
fn="$FILENAME.flac"
metaflac --set-tag=TITLE="$NAME" $fn
metaflac --set-tag=ARTIST="$ARTIST" $fn
metaflac --set-tag=Genre="$GENRE" $fn
metaflac --set-tag=Year="$YEAR" $fn
metaflac --import-picture-from=./cover.png $fn

# cleanup
rm cover.jpg
rm cover.png
rm scdm.html
