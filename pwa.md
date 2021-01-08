# Progressive Web App - PWA 

Progressive Web App is a term. It uses Service Workers and has an Application Manifest. It is used for applications that have Background Synchronisation, that are responsive, that use Web Push, geolocation, the device's camera.

- Application Manifest
- Service Workers
- Caching
- IndexedDB
- Background Sync
- Push Notifications

---
## Application Manifest
---

Allows addition to HomeScreen

```json
{
  "name":"LONG_NAME", // will be fetched by the browser and display it. i.e: Splashscreen 
  "short_name":"SHORT_NAME", // will be displayed below the icon. ie: homescreen
    "start_url": "/index.html", // which page to load on startup
    "scope":".", // here specify which files will be available as a PWA
    "display":"standalone", // should it look like a standalone app
    "background_color": "#fff",// background color of the loading/splashscreen
    "theme_color": "#eaeaea", // theme color. ie: top bar in task switcher
    "orientation":"any | portrait | portrait-primary | landscape", // set the default orientation of the screen 
    "description":"any description", // description...  
    "dir":"ltr", // read direction of the app   
    "lang":"en-US", // main language of the app   
    "related_applications":[ // if the app exists as a native app, the browser will ask to open it with it
      {
        "plateform":"play | ios",
        "url":"https://paly.google.com/store/apps/details?id=com.example.app1",
        "id":"com.example.app1"
      }
    ],  
  "icons":[ // icons for the application.ie: to be shown on homescreen
    {
    "src":"/src/images/icons/app-icon-48x48.png",
    "type":"image.png",
    "sizes":"48x48" // browser chooses best one for the given device
    },
    {
    "src":"/src/images/icons/app-icon-96x96.png",
    "type":"image.png",
    "sizes":"96x96"
    },
    {
    "src":"/src/images/icons/app-icon-144x144.png",
    "type":"image.png",
    "sizes":"144x144"
   },
    {
    "src":"/src/images/icons/app-icon-192x192.png",
    "type":"image.png",
    "sizes":"192x192"
    },
    {
    "src":"/src/images/icons/app-icon-256x256.png",
    "type":"image.png",
    "sizes":"256x256"
   },
    {
    "src":"/src/images/icons/app-icon-384x384.png",
    "type":"image.png",
    "sizes":"384x384"
    },
    {
    "src":"/src/images/icons/app-icon-512x512.png",
    "type":"image.png",
    "sizes":"512x512"
    } 
  ],
}
```

---
## Service Workers
---

Javascript file. 
Background processes attached to entire applications.  
Runs on its own thread, so parallele from the other JS files.  
They run in the background.  
They listen and react to all events.

### Events

- Fetch(http-request)  
- Push Notifications  
- Notification Interaction  
- Background Sync  
- Service Worker Lifecycle

### Lifecycle events

Install: will instal the service worker  
Activate: will activate the service worker

### Register and activate
Create a **service worker** file at the root of the application: `sw.js`  
Register the service worker file in the entire application. ie: a js file that is used by all the pages.  

```javascript
// First checking if the browser supports Service Workers
if('serviceWorker' in navigator){
  navigator.serviceWorker
   .register('/sw.js')
   .then(function(){
     console.log('Service Worker registered)
   })
}
 ```

In the **Service Worker** js file:  
```javascript
// Installing the service worker on the browser
self.addEventListener('install', function (event) {
  console.log('[serviceWorker] Installing Service Worker...', event);
})

  // Activating the service worker on the browser
self.addEventListener('activate', function (event) {
  console.log('[Service Worker] Activating Service Worker...', event);
  return self.clients.claim();
})

```

### Delaying install
To delay the display of the install banner, add code in the root js file:  

```javascript
  window.addEventListener('beforeinstallprompt', function (event) {
    event.preventDefault();
  deferredPrompt = event;
  return false;
})
```

Then in the required file (when we want to display the banner), add:  
```javascript

```

---
## Caching
---

Allows the storing of files/assets on the browser through the Service Worker.  
`key(: request): value(: response)` pair.  
`caches` // refers to the overall cache storage. Allows us to open a subcache  
`cache` // refers to a single cache  

- cache API
- Static caching/precaching
- Dynamic caching
- Removing cache
- On demand caching 
- Providing a fallback page
- Caching Strategies

### cache API

`cache.add(FILE_NAME)` : will cache one ressource only (string)  
`cache.addAll([FILE_NAME,SECOND_FILE_NAME])` : will cache an array of ressources (strings)  
`cache.match()` : will check if a ressource is cached  
`cache.delete(REQUEST_KEY)` : will delete a request (key)    

### Static caching/precaching

Caching the application shell/framework when it first loads. Static files will be cached during the **Service Worker installation**.  
Use the **cache** API.  
In the **sw.js** file:  

```javascript
// Installing the service worker on the browser
self.addEventListener('install', function (event) {
  event.waitUntil(
    caches.open('NAME_OF_THE_CACHE')
    .then(cache => {
      console.log('Precaching app shell');
      cache.add('PATH/URL'); // /index.html
      cache.add('PATH/URL'); // https://whaterever-url.com
      ...
      // or 
      cache.addAll([
        'PATH/URL',
        'PATH/URL',
        'PATH/URL'
      ])
    })
  )
})
```

The cached files will then be retrieved through the **fetch** event:   

```javascript
self.addEventListener('fetch', event => {
  event.respondWith(
    // will check for an existing request
    caches.match(event.request)
    .then(response => {
      if(response){
        // if the request exists then the browser will use it
        return response 
      } else{
        // if the request does not exist then the browser will go ahead and proceed with the request(for a file or a URL)
        return fetch(event.request);
      }
    })
  )
})
```

### Dynamic caching

Will cache the files that are not cached on first load but that are requested later. 

```javascript
self.addEventListener('fetch', function (event) {
  event.respondWith(
    caches.match(event.request)
      .then(response => {
        if (response) {
          // if the request exists then the browser will use it
          return response
        } else {
          // if the request does not exist then the browser will go ahead and proceed with the request(for a file or a URL)
          return fetch(event.request)
            .then(res => {
              // Creating a new dynamic cache
              return caches.open(CACHE_DYNAMIC_NAME)
                .then(cache => {
                  cache.put(event.request.url, res.clone())
                  return res;
                })
            })
            .catch(err => {
              return caches.open(CAHE_STATIC_NAME)
                .then(cache => {
                  return cache.match('/offline.html')
                })
            })
        }
      })
  );
});
```

### Removing cache

In the activate cycle, check for the named version of the caches. If it doesn't exist then it will be removed

```javascript
self.addEventListener('activate', (event) =>{
  event.waitUntil(
    caches.keys()
      .then(keyList => {
        // Checking for the existing caches variables
        // return Promise.all(), it will return an array of promises(here the keys), and will wait for all the promises to resolve
        return Promise.all(keyList.map(key => {
          if (key !== CACHE_STATIC_NAME && key !== CACHE_DYNAMIC_NAME) {
            console.log('removed cache:', key);
            caches.delete(key);
          }
        }))
      })
  )
  return self.clients.claim();
});
```

### On demand caching

**Not in the sw.js file**, but in any other js file, create a cache:  

```javascript
const onEvent = event => {
  console.log('event type');
  // Opening a new cache
  caches.open('USER_REQUEST')
    .then((cache) => {
      // Creating a cache
      cache.add('FILE/PATH');
      cache.add('FILE/PATH');
      // or 
      cache.addAll([
        'FILE/PATH',
        'FILE/PATH'
        ]);
    })
}
```

### Providing a fallback page

```javascript
self.addEventListener('fetch', function (event) {
  event.respondWith(

caches.match(event.request)
      .then(response => {
        if (response) {
          // if the request exists then the browser will use it
          return response
        } else {
          // if the request does not exist then the browser will go ahead and proceed with the request(for a file or a URL)
          return fetch(event.request)
            .then(res => {
              // Creating a new dynamic cache
              return caches.open(CACHE_DYNAMIC_NAME)
                .then(cache => {
                  cache.put(event.request.url, res.clone())
                  return res;
                })
            })
            .catch(err => {
              return caches.open(CACHE_STATIC_NAME)
                .then(cache => {
                  // Returns the offline page if it has been cached  
                  return cache.match('/offline.html')
                })
            })
        }
      })
  );
});
```

### Caching Strategies

- Cache only
- Network only
- Network with cache fallback
- Cache then Network

#### Cache Only

Doesn't make sense as the app will be broken and won't be able to download new content.

#### Network Only

Regular website or application. No use of Service Worker here.

#### Network with cache fallback

Rarely used as we never know how the network will behave, if it's fast or offline then cool, if not then the user might have to wait before the cache kicks in thus having a bad UX.

#### Cache then Network 

Choose which ressource we want to load and how.  

---
### IndexedDB
---

Used to **cache Dynamic Content**, usually **json data** coming from a fetch request.  

Database => Store => Data  

Import the **idb.js** file, it improves the indexedDB API. Place it in the js folder.  
Import it in the **sw.js** file at the top:  
`importScripts('/src/js/idb.js')` // change the path according to the app architecture  

Create a **utility.js** file to outsource the indexedDB code:  
**utility.js**:  

Creating an IndexedDB database
```javascript
const dbPromise = idb.open('DB_NAME', 1, (db) => {
  if (!db.objectStoreNames.contains('STORE_NAME')) {
    db.createObjectStore('STORE_NAME', { keyPath: 'id' })
  }
})
```

Storing data 
```javascript
const writeData = (st, data) => {
  console.log('write data')
  return dbPromise
    .then(db => {
      const tx = db.transaction(st, 'readwrite');
      const store = tx.objectStore(st);
      store.put(data[key]);
      return tx.complete;
    })
}
```

Retrieving data
```javascript
const readAllData = (st) => {
  return dbPromise
    .then(db => {
      const tx = db.transaction(st, 'readonly');
      const store = tx.objectStore(st);
      return store.getAll();
    })
}
```

Deleting all the indexedDB data    
```javascript
function clearAllData(st) {
  return dbPromise
    .then(function (db) {
      var tx = db.transaction(st, 'readwrite');
      var store = tx.objectStore(st);
      store.clear();
      return tx.complete;
    });
}
```

Removing one item
```javascript
function deleteItemFromData(st, id) {
  dbPromise
    .then(function (db) {
      var tx = db.transaction(st, 'readwrite');
      var store = tx.objectStore(st);
      store.delete(id);
      return tx.complete;
    })
    .then(function () {
      console.log('Item deleted!');
    });
}
```

---
## Background Sync
---

Will store data and send it straight away if the network is up or when it comes back on.  

In the **sw.js** file:  

```javascript
self.addEventListener('sync', function(event) {
  console.log('[Service Worker] Background syncing', event);
  if (event.tag === 'sync-new-posts') {
    console.log('[Service Worker] Syncing new Posts');
    event.waitUntil(
      readAllData('sync-posts')
        .then(function(data) {
          for (var dt of data) {
            fetch('https://us-central1-pwagram-99adf.cloudfunctions.net/storePostData', {
              method: 'POST',
              headers: {
                'Content-Type': 'application/json',
                'Accept': 'application/json'
              },
              body: JSON.stringify({
                id: dt.id,
                title: dt.title,
                location: dt.location,
                image: 'https://firebasestorage.googleapis.com/v0/b/pwagram-99adf.appspot.com/o/sf-boat.jpg?alt=media&token=19f4770c-fc8c-4882-92f1-62000ff06f16'
              })
            })
              .then(function(res) {
                console.log('Sent data', res);
                if (res.ok) {
                  res.json()
                    .then(function(resData) {
                      deleteItemFromData('sync-posts', resData.id);
                    });
                }
              })
              .catch(function(err) {
                console.log('Error while sending data', err);
              });
          }

        })
    );
  }
});

```


In the case of a form submit:  
```javascript
form.addEventListener('submit', function (event) {

  // Preventing the page to reload
  event.preventDefault();

  // Making sure the inputs values are valid
  if (titleInput.value.trim() === '' || locationInput.value.trim() === '') {
    alert('Please enter valid data!');
    return;
  }

  // Checking if the service worker and sync are supported by the browser
  if ('serviceWorker' in navigator && 'SyncManager' in window) {
    navigator.serviceWorker.ready
      .then(function (sw) {
        var post = {
          id: new Date().toISOString(),
          title: titleInput.value,
          location: locationInput.value
        };
        writeData('sync-posts', post)
          .then(function () {
            return sw.sync.register('sync-new-posts');
          })
          .then(function () {
            var snackbarContainer = document.querySelector('#confirmation-toast');
            var data = { message: 'Your Post was saved for syncing!' };
            snackbarContainer.MaterialSnackbar.showSnackbar(data);
          })
          .catch(function (err) {
            console.log(err);
          });
      });
  } else {
    sendData();
  }
});
```

---
## Push Notifications
---

Displaying a pop up on the user's screen.

- Enable notifications

### Enable notification
First we ask the user to enable notification, otherwise nothing will work afterwards

In any JS file:  
```javascript
function displayConfirmNotification(){
  if('serviceWorker'in navigator){

    // Note that not ALL options will be available depending on the device
    const options = {
      body:'You successfully subscribed to our Notification Service',
      icon:'/src/images/icons/app-icon-96x96.png',
      image:'/src/images/...',
      dir: 'ltr',
      lang:'en-US', 
      vibrate: [100, 50, 200], // vibration / pause / vibration / pause / .....
      badge: '/src/images/icons/app-icon-96x96.png',
      tag: 'confirm-notification', // if set will stack the notifications, they won't show
      renotify: boolean,
      actions: [
        {action: 'confirm', title: 'Okay', icon: '/src/images/icons/app-icon-96x96.png'},
        {action: 'cancel', title: 'Cancel', icon: '/src/images/icons/app-icon-96x96.png'}
      ]
    };

    navigator.serviceWorker.ready
    .then(swreg => {
      swreg.showNotification('Successfully subscribed from sw!', options);
    })
  }
}

function configurePushSub(){
  if(!('serviceWorker' in navigator)){
    return;
  }

  let reg;
  navigator.serviceWorker.ready
  .then(swreg => {
    return swreg.pushManager.getSubscription()
  })
  .then(sub => {
    if(sub === null){
      reg.pushManager.subscribe({
        userVisibleOnly: true,
      })
    }else{

    }
  })
}

function askForNotificationPermission() {
  Notification.requestPermission((result) =>{
    console.log('User Choice', result);
    if (result !== 'granted') {
      // Here the user either didnt accept or didnt respond,
      // if he didn't respond then it will be asked again
      // if he didnt accept there's nothing more we can do...
      console.log('No notification permission granted!');
    } else {
      // The user agreed. We can hide the enable notification button
      configurePushSub()
      // displayConfirmNotification();
    }
  });
}

// Checking if the browser supports notifications 
if ('Notification' in window) {
  // Do something then like showing a 'enable notification' button
    enableNotificationsButton.style.display = 'inline-block';
    enableNotificationsButton.addEventListener('click', askForNotificationPermission);
  }
}
```

In the **sw.js** file
```javascript
self.addEventListener('notificationclick', (event) => {
  const notification = event.notification;
  const action = notification.action;

  console.log(notification);

  if(action === 'confirm'){
    console.log('Confirm was chosen');
  }else{
    console.log(action);
  }
})

self.addEventListener('notificationclose', (event) => {
  console.log('notification was closed');
})
```

### VAPID keys

In the **backend** files:  
`npm i --save web-push`

Add a new script in the **package.json** file:  
`"web-push": "web-push"`  

Then run `npm run web-push generate-vapi-keys` to generate a public and a private *vapi key*