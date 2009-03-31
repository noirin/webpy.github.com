---
layout: default
title: Store an uploaded file
---

# Store an uploaded file

## Problem

You want to upload a file and store it in a predefined folder.

## Solution

    import web
    
    urls = ('/upload', 'Upload')
    
    class Upload:
        def GET(self):
            return """<html><head></head><body>
    <form method="POST" enctype="multipart/form-data" action="">
    <input type="file" name="myfile" />
    <br/>
    <input type="submit" />
    </form>
    </body></html>"""
    
        def POST(self):
            x = web.input(myfile={})
            filedir = '/path/where/you/want/to/save'
            if 'myfile' in x: # to check if the file-object is created
                filepath=x.myfile.filename.replace('\\','/') # replaces the windows-style slashes with linux ones.
                filename=filepath.split('/')[-1] # splits the and chooses the last part (the filename with extension)
                fout = file(filedir +'/'+ filename,'w') # creates the file where the uploaded file should be stored
                fout.write(x.myfile.file.read()) # writes the uploaded file to the newly created file.
                fout.close() # closes the file, upload complete.
            raise web.seeother('/upload')


    if __name__ == "__main__":
       app = web.application(urls, globals()) 
       app.run()

## Hang ups

A couple of things to watch out for:

* See [fileupload](/fileupload).
* Don't put the file in a folder that is executable without any check of the extension and type of file.