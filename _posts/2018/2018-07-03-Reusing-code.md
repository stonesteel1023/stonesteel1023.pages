---
layout: post
title:  "Reusing-code"
date:   2018-07-03 09:00:00 +0900
categories: coding
tag:
- JavaScript
comments : true
---

# 코드 재사용


```javascript

const cache = () => {
  const store = {}

  const set = (key, value) => {
    store[key] = value
  }
  const remove = key => {
    const value = store[key]
    delete store[key]
    return value
  }
  return { set, remove }
}

// Let's use the cache
const simpleCache = cache()

simpleCache.set('a', 1)
simpleCache.set('b', 2)
simpleCache.set('b', 3)

console.log(simpleCache.remove('a')) // 1
console.log(simpleCache.remove('b')) // 3
console.log(simpleCache.remove('b')) // undefined


--------------------------------------------------------------------------------------------------
// 메소드 통채로 복사해서 재사용

const cache = () => {
  const store = {}

  const set = (key, value) => {
    store[key] = value
  }

  const remove = key => {
    const value = store[key]
    delete store[key]
    return value
  }

  return { set, remove }
}

const expire = (cache, ttl, expirationHandler) => {
  const timers = {}

  const set = (key, value) => {
    // Store value
    cache.set(key, value)
    // Clear existing timer
    clearTimeout(timers[key])
    // Set expiration timer
    timers[key] = setTimeout(() => {
      const value = cache.remove(key)
      delete timers[key]
      expirationHandler(key, value)
    }, ttl)
  }

  const remove = key => {
    clearTimeout(timers[key])
    delete timers[key]
    return cache.remove(key)
  }

  return { set, remove }
}

// Let's use the simple cache
const simpleCache = cache()

simpleCache.set('a', 1)
simpleCache.set('b', 2)
simpleCache.set('b', 3)

console.log(simpleCache.remove('a')) // 1
console.log(simpleCache.remove('b')) // 3
console.log(simpleCache.remove('b')) // undefined

// Let's use the expiring cache
const expirationHandler = (key, value) => {
  console.log(`expired ${key}: ${value}`)
}
const expiringCache = expire(cache(), 1000, expirationHandler)

expiringCache.set('a', 1)
expiringCache.set('b', 2)

console.log(expiringCache.remove('a')) // 1
console.log(expiringCache.remove('a')) // undefined
setTimeout(() => {
  console.log(expiringCache.remove('b')) // undefined
}, 1100)

```
