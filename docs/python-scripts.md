---
sidebar_position: 1
---

# სკრიპტები

მოცემული სკრიპტების გასაშვებად უმჯობესია გამოყენებულ იქნეს
[Anaconda](https://www.anaconda.com/), მასში შედის
**jupyter**, რომელიც საშუალებას გვაძლევს პითონის სკრიპტები გავუშვათ
არჩეულ ფოლდერში.

Anaconda ს ინსტალაციის შემდგომ შეგიძლიათ ვინდოუსის საძიებო
ველში ჩაწეროთ **jupyter**, დააკლიკოთ ენთერს, რის შემდგომაც ბრაუზერში
გაიხსნება ფანჯარა რომელშიც მოცემული იქნება თვქენი ფაილური სისტემა.
შესაბამისი ფოლდერის არჩევის შემდგომ გახსნით ახალ სამუშაო ფანჯარას.

![screenshot](/img/products_screens/kernel.png)

### ფოტოების გაკვადრატება

```bash
import os
from tqdm.notebook import tqdm
from PIL import Image
import os
import PIL
import glob

path = "imgs/750"
dirs = os.listdir( path )

def expand2square(pil_img, background_color):
    width, height = pil_img.size
    if width == height:
        return pil_img
    elif width > height:
        result = Image.new(pil_img.mode, (width, width), background_color)
        result.paste(pil_img, (0, (width - height) // 2))
        return result
    else:
        result = Image.new(pil_img.mode, (height, height), background_color)
        result.paste(pil_img, ((height - width) // 2, 0))
        return result
    
for item in tqdm(dirs):
    if 'webp' in item:
        continue
    image = Image.open(path+'/'+item)
     #new_im.save(, 'JPEG', quality=90)
    im_new = expand2square(image, (255, 255, 255))
    name = item.split('.')[0]
    os.makedirs(f'imgs/square', exist_ok=True)
    im_new.save(f'imgs/square/' + name+".jpg", "JPEG")
```
### სახელების გადარქმევა

```bash
import os
from tqdm.notebook import tqdm
from PIL import Image

path = "main"
dirs = os.listdir( path )
sizes = [250, 750, 2000]

def get_name(item):
    name = item.split('.')[0]
    if not '_' in name:
        name = f"{name}_1"
    else:
        sort = int(name.split('_')[-1])
        name = f"{name}_{sort+1}"
       
    name = f"{name}.jpg"
    return name

def resize_aspect_fit():
    
    for size in sizes:
        os.makedirs(f'imgs/{size}', exist_ok=True)

        for item in tqdm(dirs):
            if 'webp' in item:
                continue
            image = Image.open(path+'/'+item)
            image.thumbnail((size, size), Image.ANTIALIAS)
            image = image.convert("RGB")
            name = get_name(item)
            image.save(f'imgs/{size}/' + name, "JPEG")
             #new_im.save(, 'JPEG', quality=90)
            
resize_aspect_fit()


/ *   IF PROBLEM WITH 0 Bytes
Image files 0 byte 
from PIL import ImageFile
ImageFile.LOAD_TRUNCATED_IMAGES = True
```

### ლინკებიდან გადმოწერა

```bash
import pandas as pd

df = pd.read_excel('biblusi_import.xlsx', sheet_name='Sheet1')

import os
import requests
from PIL import Image
from tqdm.notebook import tqdm

for i, row in tqdm(df.iterrows(), total=len(df)):
    name = row['code']
    name = f"{name}_1"
   
    if os.path.exists(f'imgs/main/{name}.jpg'):
        continue
    path = row['link1']
    try:
        f = open(f'imgs/main/{name}.jpg','wb')
        f.write(requests.get(path).content)
        f.close()
    except Exception as e:
        print(f"Invalid link: {path}\n sku: {name}")
        print(e)
```

### ფოლდერებიდან ფოტოების ამოღება

```bash
import os
import glob
from os import listdir

rootdir = 'imgs/main'
for folder in os.listdir(rootdir):
    d = os.path.join(rootdir, folder)
    x = 0
    tifCounter = len(glob.glob1(f'imgs/main/{folder}',"*.jpg"))
    while x < tifCounter:
        for file in os.listdir(f'imgs/main/{folder}'):
            list = os.listdir(f'imgs/main/{folder}')
            x += 1
            print(file)
            name = folder.replace(' ', '')
            if ".jpg" in file:
                old_file_name = f'imgs/main/{folder}/{file}'
                new_file_name = f'imgs/main2/{name}_{x}.jpg'
                os.rename(old_file_name,new_file_name)
        x += 1

```

### როცა ფოტოებს არ აქვთ სორტი _1

```bash
import os
from tqdm.notebook import tqdm
from PIL import Image

path = "imgs/main"
dirs = os.listdir( path )
sizes = [250, 750, 2000]

def get_name(item):
    name = item.split('.')[0]
    if not '_' in name:
        name = f"{name}_1"
    else:
        sort = int(name.split('_')[-1])
        name = name.split('_')[0]
        name = f"{name}_{sort+1}"
       
    name = f"{name}.jpg"
    return name

def resize_aspect_fit():
    
    for size in sizes:
        os.makedirs(f'imgs/{size}', exist_ok=True)

        for item in tqdm(dirs):
            if 'webp' in item:
                continue
            image = Image.open(path+'/'+item)
            image.thumbnail((size, size), Image.ANTIALIAS)
            image = image.convert("RGB")
            item = item.replace('.JPG', '.jpg')
            name = get_name(item)
            image.save(f'imgs/{size}/' + name, "JPEG")
             #new_im.save(, 'JPEG', quality=90)
            
resize_aspect_fit()
```