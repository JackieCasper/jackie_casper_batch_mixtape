# Batch Update Mixtape

## Overview
A console application that applies a batch of changes to a mixtape file to create an updated output file. The following types of changes can be made:

- Add an existing song to an existing playlist.
- Add a new playlist for an existing user; the playlist should contain at least one existing song.
- Remove an existing playlist.

## Setup
1. Clone this repo wherever you see fit
2. Add the directory to PATH. You can either add it to your `~/.bash_profile` or export it directly in terminal using:
```bash
export PATH=$PATH:~/your/path/here/jackie_casper_batch_mixtape
```

## Usage
### Running the Command

The command can be run in the console from any location with the following format:

```bash
batch-update-mixtape <input-file> <changes-file> <output-file>
```
For example:
```bash
batch-update-mixtape mixtape.json changes.json output.json
```

It takes 3 arguments:

1. The name of the input file
2. The name of the changes file
3. The name of the output file

Examples have been provided for the `mixtape.json` and `changes.json`

### Changes File Structure

The changes file should be a json with the following structure:

```json
{
  "playlists": {
    "create": [
      { 
        "user_id": "1",
        "song_ids": ["1"]
      }
    ],
    "update": {
      "1": {
        "song_ids": ["1"]
      }
    },
    "delete": ["2"]
  }
}
```

All updates should be under the `playlists` key which contains an object of the types of changes to be made.

#### Create
Add a new playlist for an existing user; the playlist should contain at least one existing song. The value should be an array of objects with the following keys:
- `user_id`: string of the id associated to an existing user 
- `song_ids`: array of string ids associated to existing songs

_**Notes:**_
- _The new playlists's ids are assigned based off of the largest existing playlist id._
- _If there is no user with the given user_id the playlist will not be created._
- _If there are no song ids, or if the song ids do not associate to any existing songs, the playlist will not be created._
- _If there is a mix of valid song ids and invalid song ids the playlist will be created with only the valid ids._

#### Update
Add an existing song to an existing playlist. The value should be an object where:
- the **id** of any updated song is the **key** 
- the value is an object that contains the **song_ids** to be added as an array.

_**Notes:**_
- _The structure allows for easy lookup by id and for other properties (mainly user_id) to be updated if needed in the future._
- _If there is no existing song with a given id, it will not be added to the playlist._
- _If there is no playlist with the given id, no changes will occur_

#### Delete
Remove an existing playlist. Consists of an array with the ids of playlists to be deleted.


## Future Improvements
### Scalability
When working with large files a major consideration is memory usage. Currently this command reads the entire files into memory. To accommodate large files I would consider using a stream to parse chuncks of the file at a time and a cache to store any persistent data needed to execute the task. 

### Known Issues
- Inputting incorrect file names will throw an error
- Incorrectly formatted json will throw an error