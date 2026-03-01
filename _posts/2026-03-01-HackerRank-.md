---
layout: single
title: "HackerRank - Time Conversion"
date: 2026-03-01
categories: [CodingTest]
tags: [C++, HackerRank]
toc: true
toc_sticky: true
---

## (Warmup) Time Conversion

URL : [https://www.hackerrank.com/challenges/time-conversion/problem](https://www.hackerrank.com/challenges/time-conversion/problem?isFullScreen=true)

(설명)
- AM, PM 으로 되는 12시간계 시계를, 24시간계로 변환하는 문제
- 시, 분, 초가 있을때, 시에 해당하는 부분만 변환하면 되는 문제

```cpp
#include <bits/stdc++.h>

using namespace std;

/*
 * Complete the 'timeConversion' function below.
 *
 * The function is expected to return a STRING.
 * The function accepts STRING s as parameter.
 */

string timeConversion(string s) {
    string result = "";
    string temp = s.substr(0, 2);
    int hour = stoi(temp);

    if (s.find("PM") != string::npos
        && hour < 12)
    {
        hour += 12;
    }
    else if(s.find("AM") != string::npos
            && hour >= 12)
    {
        hour -= 12;
    }
    result = to_string(hour);
    
    if (result.length() == 1)
        result = "0" + result;
    
    return result += s.substr(2, 6);
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));

    string s;
    getline(cin, s);

    string result = timeConversion(s);

    fout << result << "\n";

    fout.close();

    return 0;
}
```