# Snapchat Memories Download

## Purpose

Snapchat has moved away from its 'unlimited memories' feature and has no added a paywall in order to keep more than 5GB of memories... It was a long time coming.

I was incredibly not bothered to pay the subscription, and the only reason I hadn't downloaded them sooner was because any time I downloaded them the file names were a series of random characters, and they had no sort order so I wasn't able to scroll through my memories and reminisce or find any particular memory by knowing the date (first world problems).

I got in touch with snapchat support to ask if they could provide my memories (saved images/videos) with some embedded metadata like a timestamp, or even if there was a possibility of downloading them with the filenames as timestamps, but they said there was no way to make that happen.

## Function

This repo contains a python notebook/script (haven't decided yet) that will read the memories_history.json file that you should receive when you download your memories via the Snapchat website.

The script will then read each memory, which is formatted as:
```json
{
"Date": "2018-12-30 01:04:12 UTC",
"Media Type": "Image",
"Location": "Latitude, Longitude: 50.755175010750555, -2.228376121665392",
"Download Link": "https://app.snapchat.com/dmd/memories?uid=xxx&sid=xxxmid=xxx&ts=xxx&proxy=true&sig=xxx",
"Media Download Url": "https://us-east1-aws.api.snapchat.com/dmd/mm?uid=xxx&sid=xxx&mid=xxx&ts=xxx&sig=xxx"
}
```

The URL from the above memory is then accessed and the media is downloaded with the file name as a timestamp (YYYY-MM-DD_H-M-S). There is some extra logic to include a suffix if there is more than one file with the same timestamp.

Sometimes, if the memory contains extra graphics like captions/stickers, these are stored separately, so the image and extra graphics are stored in a ZIP folder instead of a standalone media file. Unfortunately I am not bothered enough to overlay the graphics over the original image as part of this work so the best I could do was just to save them with the suffix 'main' or 'overlay' so that this processing could be done at a later stage.


## Input

You will need to provide to the script:
- <i> JSON_FILE </i>: The path to the memories_history.json file that you downloaded from snapchat
- <i> output_dir </i>: The path to the folder where you want the renamed files to be saved


## Output

- A folder containing all listed memories labelled by timestamp in the form YYYY-MM-DD_HHMMSS (+suffixes for duplicates/main/overlay images)
- A subfolder containing the original zip files (media with extra graphics) to be deleted at a later stage if you're happy with the main foldere

## Prerequisites/Dependencies

You will need to download your data from Snapchat (instructions can be found [here](https://help.snapchat.com/hc/en-us/articles/7012305371156-How-do-I-download-my-data-from-Snapchat)). 
You must select BOTH 'Export your memories' and 'Export JSON Files'. You muse tick that you want to download the memories themselves (even though the script doesn't use them) because it's the only way the memories_history.json file gets generated. 

You also must set up your environment, I have a conda environment to manage dependencies.

If you use conda too, you can set up the environment by running the first line, then activate it by running the second line:

```bash
conda env create -f environment.yml
conda activate SnapchatMemoriesDownload
```


Otherwise, AI told me this is how you would do it:
```bash
python -m venv venv
source venv/bin/activate   # (Mac/Linux)
venv\Scripts\activate      # (Windows)

pip install requests
```
