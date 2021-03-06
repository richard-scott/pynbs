# pynbs

[![PyPI](https://img.shields.io/pypi/v/pynbs.svg)](https://pypi.org/project/pynbs/)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pynbs.svg)](https://pypi.org/project/pynbs/)

> A simple python library to read and write [.nbs files](http://www.stuffbydavid.com/mcnbs/format)
from [Note Block Studio](http://www.stuffbydavid.com/mcnbs). Compatible with
python 2 and 3.

`pynbs` makes it possible to easily iterate over Note Block Studio songs.

```python
import pynbs

for tick, chord in pynbs.read('demo_song.nbs'):
    print(tick, [note.key for note in chord])
```

You can also use `pynbs` to generate new songs programmatically.

```python
import pynbs

new_file = pynbs.new_file(song_name='Hello world')
new_file.notes.extend([
    pynbs.Note(tick=i, layer=0, instrument=0, key=i + 35) for i in range(10)
])

new_file.save('new_file.nbs')
```

## Installation

The package can be installed with `pip`.

```bash
$ pip install pynbs
```

## Basic usage

### Reading files

You can use the `read()` function to read and parse a specific nbs file.

```python
demo_song = pynbs.read('demo_song.nbs')
```

The `read()` function returns a `pynbs` file object. These objects have several
attributes that mirror the binary structure of nbs files.

#### Header

The first attribute is `header`, the file header. It contains information about
the file.

```python
header = demo_song.header
```

Attribute                   | Type    | Details
:---------------------------|:--------|:------------------------------------------------
`header.song_length`        | `int`   | The length of the song, measured in ticks.
`header.song_layers`        | `int`   | The id of the last layer with at least one note block in it.
`header.song_name`          | `str`   | The name of the song.
`header.song_author`        | `str`   | The author of the song.
`header.original_author`    | `str`   | The original song author of the song.
`header.description`        | `str`   | The description of the song.
`header.tempo`              | `float` | The tempo of the song.
`header.auto_save`          | `bool`  | Whether auto-saving has been enabled.
`header.auto_save_duration` | `int`   | The amount of minutes between each auto-save.
`header.time_signature`     | `int`   | The time signature of the song.
`header.minutes_spent`      | `int`   | The amount of minutes spent on the project.
`header.left_clicks`        | `int`   | The amount of times the user have left clicked.
`header.right_clicks`       | `int`   | The amount of times the user have right clicked.
`header.blocks_added`       | `int`   | The amount of times the user have added a block.
`header.blocks_removed`     | `int`   | The amount of times the user have removed a block.
`header.song_origin`        | `str`   | The file name of the original midi or schematic.

> For more information about all these fields, check out the [official specification](http://www.stuffbydavid.com/mcnbs/format).

#### Notes

The `notes` attribute holds a list of all the notes of the song in order.

```python
first_note = demo_song.notes[0]
```

Attribute         | Type  | Details
:---------------- |:------|:------------------------------------------------
`note.tick`       | `int` | The tick at which the note plays.
`note.layer`      | `int` | The id of the layer in which the note is placed.
`note.instrument` | `int` | The id of the instrument.
`note.key`        | `int` | The key of the note. (between 0 and 87)

#### Layers

The `layers` attribute holds a list of all the layers of the song in order.

```python
first_layer = demo_song.layers[0]
```

Attribute         | Type  | Details
:-----------------|:------|:------------------------
`layer.id`        | `int` | The id of the layer.
`layer.name`      | `str` | The name of the layer.
`layer.volume`    | `int` | The volume of the layer.

#### Instruments

The `instruments` attribute holds a list of all the custom instruments of the
song in order.

```python
first_custom_instrument = demo_song.instruments[0]
```

Attribute              | Type   | Details
:----------------------|:-------|:----------------------------------------------------------
`instrument.id`        | `int`  | The id of the instrument.
`instrument.name`      | `str`  | The name of the instrument.
`instrument.file`      | `str`  | The name of the sound file of the instrument.
`instrument.pitch`     | `int`  | The pitch of the instrument. (between 0 and 87)
`instrument.press_key` | `bool` | Whether the piano should automatically press keys with the instrument when the marker passes them.

### Iterating over songs

Iterating over a `pynbs` file object yields consecutively all the chords of the song with
the associated tick.

```python
for tick, chord in demo_song:
    ...
```

`chord` is a list of all the notes that play during the tick `tick`.

### Creating new files

You can create new files using the `new_file()` function. The function lets
you specify header attributes with keyword arguments.

```python
new_file = pynbs.new_file(song_name='Hello world')
```

The function returns a new `pynbs` file object that you can now edit
programmatically.

### Saving files

You can use the `save()` method to  encode and write the file to a specified
location.

```python
new_file.save('new_file.nbs')
```

---

License - [MIT](https://github.com/vberlier/pynbs/blob/master/LICENSE)
