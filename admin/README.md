# Admin scripts


## List processes by memory size

```python
import psutil
import argparse

# list processes with memory usage above the 
# given value in MB

parser = argparse.ArgumentParser()
parser.add_argument('memory', help='memory value in MB')

args = parser.parse_args()

mem_limit = int(args.memory) * 1024 * 1024

p_it = psutil.process_iter(['name', 'memory_info'])

large_mem_ps = [{'pid': p.pid, 'name': p.info['name'], 'mem': p.info['memory_info'].rss}
                for p in p_it if p.info['memory_info'].rss > mem_limit]

for p in large_mem_ps:

    print(f'{p['pid']} {p['name']} {(p['mem'] / (1024 * 1024)):.2f}')
```

## List processes

Using `psutil` to list processes and `rich` to format data into console table.  
Currently, rich does not support summary info in the footer.  

The `-a` and `-n` options are mutually exclusive; either we list all processes or  
only a specific process.  

If we provide both short and long options, such as `-a` and `-all`, the parser stores  
the name of the long option.    

```python
import psutil
import argparse
from datetime import datetime
from rich import box
from rich.console import Console
from rich.table import Table
from datetime import date


def parse_arguments():
    parser = argparse.ArgumentParser()
    group = parser.add_mutually_exclusive_group(required=True)
    group.add_argument('-a', '--all', action='store_true',
                       help='show all processes')
    group.add_argument('-n', '--name', help='show info about process name')
    args = parser.parse_args()
    return args.all, args.name


def list_process(name):
    now = f'{date.today()}'
    table = Table(title=f'Process', box=box.MINIMAL,
                  caption=now, caption_justify='left')
    table.add_column('id', style='cyan')
    table.add_column('process name', style='grey69')
    table.add_column('username')
    table.add_column('create time', style='blue')
    table.add_column('memory', style='green')

    process_count = 0

    for p in psutil.process_iter():

        if name in p.name().lower():
            ctime = datetime.fromtimestamp(p.create_time())
            memory_percent = p.memory_percent()
            table.add_row(f'{p.pid}', p.name(), p.username(),
                          ctime.isoformat(), f'{memory_percent:.2f}')
            process_count += 1

    if process_count > 0:

        console = Console()
        console.print(table, justify='center')
    else:

        print('no such process found')


def list_all_processes():

    now = f'{date.today()}'
    table = Table(title='Processes', box=box.MINIMAL,
                  caption=now, caption_justify='left')
    table.add_column('id', style='cyan')
    table.add_column('process name', style='grey69')

    pnames = []
    for p in psutil.process_iter():
        
        pnames.append(p.name())
        table.add_row(f'[bold]{p.pid}', f'[bold]{p.name()}')

    console = Console()
    console.print(table, justify='center')

    print(len(pnames), 'processes')
    print(len(set(pnames)), 'apps')


all_f, name = parse_arguments()

if all_f:
    list_all_processes()
elif name:
    list_process(name)
```



## Resize images

```python

import argparse
import pathlib
from PIL import Image
import os, re

def parse_args():

    parser = argparse.ArgumentParser()

    parser.add_argument('-s', default='images', help='source directory')
    parser.add_argument('-d', default='resized_images', help='destination directory')

    args = parser.parse_args()

    return args.s, args.d

def create_dest_dir(dir_name):

    path = pathlib.Path(dir_name)

    if not path.exists():
        path.mkdir()
        
    return path

def get_image_names(dir_name):

    path = pathlib.Path(dir_name)
    file_names = path.glob('*')

    pattern = re.compile(r'.*\.(jpg|jpeg|png)')
    return [fname for fname in file_names if re.fullmatch(pattern, fname.as_posix())] 

def resize_images(path, names):

    for name in names:
        img = Image.open(name).copy()
        img.thumbnail((400, 300))
        img.save(path / os.path.basename(name))

src_dir, dest_dir = parse_args() 

image_fnames = get_image_names(src_dir)
path = create_dest_dir(dest_dir)
resize_images(path, image_fnames)
```

## find/zip files 

```python
import pathlib
import argparse
import zipfile
import tempfile
import time


parser = argparse.ArgumentParser()
   
parser.add_argument('-p', action='store_true', help='print files')
parser.add_argument('-t', required=True, help='file type')
parser.add_argument('-d', help='search directory')
parser.add_argument('-z', action='store_true', help='archive files')
args = parser.parse_args()

p = args.p
z = args.z
file_type = args.t
search_dir = args.d

if not search_dir:
    path = pathlib.Path.home() / 'Documents'
else:
    path = pathlib.Path(search_dir)


n_of_files = 0
archive_files = []

print(f'searching for {file_type} files in {path}')

for fname in path.rglob(f'*.{file_type}'):

    if fname.is_dir():
        continue

    n_of_files += 1

    if z:
        archive_files.append(fname)

    if p:
        print(fname)


print(f'there are {n_of_files} {file_type} files in {path}')


if z:
    time_stamp = str(int(time.time()))
    temp_path = pathlib.Path(tempfile.gettempdir()) / f'archive-{file_type}-{time_stamp}.zip' 

    with zipfile.ZipFile(temp_path, 'w') as zip:

        for fname in archive_files:
            zip.write(fname)

        print(f'{temp_path} created')
```

## find large files 

The `try/except` is used within the for loop in order not to terminate the script  
when we cannot access the file.  

```python
import pathlib
import argparse


def process_args():

    gigabyte = 1024*1024*1024

    parser = argparse.ArgumentParser()
    
    parser.add_argument('-p', action='store_true', help='print files')
    parser.add_argument('-s', type=int, default=gigabyte,  help='file size in bytes')
    parser.add_argument('-d', help='search directory')

    args = parser.parse_args()
    search_dir = args.d

    if not search_dir:
        path = pathlib.Path.home() / 'Documents'
    else:
        path = pathlib.Path(search_dir)

    return path, args.p, args.s


def should_skip(fname):

    excludes = ['CanonicalGroupLimited']

    for exclude in excludes:
        if exclude in fname.absolute().as_posix():
             return True 
        
    return False


def search_for_large_files(should_print, fsize_limit):

    n_of_files = 0

    for fname in path.rglob('*'):

        if should_skip(fname):
            continue

        try: 
            if fname.is_file():

                fsize = fname.stat().st_size

                if fsize >= fsize_limit:
                    n_of_files += 1

                    if should_print:
                        print(f'{fname} has {fsize:,} bytes')

        except OSError as e:
                print(e)

    return n_of_files


path, should_print, fsize_limit = process_args()
print(f'searching for files larger than {fsize_limit} bytes in {path}')

n_of_files = search_for_large_files(should_print, fsize_limit)
print(f'there are {n_of_files} files larger than {fsize_limit} in {path}')
```

The `stat` and `is_file` or `is_dir` methods are problematic in that they can  
throw exception if they cannot access the file. For instance, the method cannot  
follow a symlink or cannot access the due to permissions.  

```python
def should_skip(fname):

    excludes = ['CanonicalGroupLimited']

    for exclude in excludes:
        if exclude in fname.absolute().as_posix():
             return True 
        
    return False
```

We put the directories that we cannot access into the excludes list and do not  
process them.  
