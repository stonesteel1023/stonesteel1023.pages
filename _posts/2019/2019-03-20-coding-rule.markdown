---
layout: post
date : 2019-03-20 10:00
title : "Coding-rule"
comments : true
tag : java
---

# Code refactoring
```java
public String getMax(List<String> list) {
    if (list != null) {　・・・　①②
        if (!list.isEmpty()) {　・・・　①②

            String max = null;
            for (String str : list) {

                if (str.contains(CHECK_STR)) {　・・・　②
                    if (max != null) {　・・・　③
                        if (str.compareTo(max) > 0) {　・・・　③
                            max = str;
                        }
                    } else {　・・・　③
                        max = str;
                    }
                }
            }

            return max;

        } else {
            return null;
        }

    } else {
        return null;
    }
}
```

## Simplified improvement of nested programs
▪ Improvements
① Common processing is cut out as common processing.
② At first, values come whichi is not subject to processing.
③ Review conditional expressions

```java
public String getMax(List<String> list) {
    if (this.isEmpty(list)) {　・・・　②
        return null;
    }

    String max = null;
    for (String str : list) {
        if (!str.contains(CHECK_STR)) {　・・・　②
            continue;
        }

        if (max == null || str.compareTo(max) > 0) {　・・・　③
            max = str;
        }
    }
	return max;

}

private boolean isEmpty(List<String> list) {　・・・　①
    boolean empty = (list == null || list.isEmpty());
    return empty;
}

```
