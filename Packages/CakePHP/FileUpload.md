---
title: 🆙 File Upload plugin
description: File upload plugin for CakePHP
published: true
date: 2020-09-11T20:52:36.296Z
tags: cakephp, composer, cakephp-plugin, php
editor: markdown
---

# 🆙 FileUpload plugin for CakePHP

## Installation

You can install this plugin into your CakePHP application using [composer](https://getcomposer.org).

The recommended way to install composer packages is:

## 🐘 From packagist

```
composer require maymeow/file-upload
```

## 🛡 From my server

Add to `composer.json` following repository

```json
{"type":"composer","url":"https://git.cloud.hsoww.net/api/v4/group/121/-/packages/composer/packages.json"}
```

then run

```
composer require maymeow/file-upload
```

## Usage

To load plugin addd to `Application.php`

```php
$this->addPlugin('FileUpload');
```

Next load it in controllers where you want to use it. To use default config

```php
public function initialize(): void
{
    parent::initialize();
    $this->loadComponent('FileUpload.Upload');
}
```

Or you can change them

```php
public function initialize(): void
{
    parent::initialize();
    $this->loadComponent('FileUpload.Upload', [
        'fieldName' => 'your_form_file_field',
        'storagePath' => 'path_to_storage',
        'allowedFileTypes' => '*'
    ]);
}
```

For `allowedFileTypes` use `'allowedFileTypes' => '*'` for all file types or `'allowedFileTypes' => ['type1', 'type2']` for your expected file types. If file have not allowed type Component will throw `Cake\Http\Exception\HttpException`.

Usage in function

```php
public function add()
{
    $file = $this->Files->newEmptyEntity();

    if ($this->request->is('post')) {
        
        // you can use try catch to show Flash instead of error page
        try {
            // Get Uploaded file details
            $fileObject = $this->Upload->getFile($this->request);   

            $file = $this->Files->patchEntity($file, $this->request->getData());

            // Update Model and write it
            $file->name = $fileObject->getClientFilename();
            $file->size = $fileObject->getSize();
            $file->path = $fileObject->getClientFilename();
            $file->updated = Date::now();

            if ($this->Files->save($file)) {
                $this->Flash->success(__('The file has been saved.'));

                return $this->redirect(['action' => 'index']);
            }
            $this->Flash->error(__('The file could not be saved. Please, try again.'));

        } catch (HttpException $e) {
            $this->Flash->error($e->getMessage());
        }
    }
    $this->set(compact('file'));
}
```

## 🎯 Direction

* [x] Configurable field name
* [x] Configurable path to storage
* [x] Allowed file types
* [ ] Multiple file upload
* [ ] File Size
* [ ] Factory with most common file types

💡 If you have more ideas then you can post issue on porect's GitHub page.

## License

MIT