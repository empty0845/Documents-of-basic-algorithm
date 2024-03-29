```python
'''
这是一个简单的文本比对，比对规则不一定科学，自行修改。堆列表排序可换成优先队列，提高效率
'''
def compareTextWithAnswer(text, ans):
    score = 0
    dtText = dict()
    dtAnswer = dict()
    for i in range(len(text)):
        if text[i] in dtText:
            dtText[text[i]].append(i)
        else:
            dtText[text[i]] = [i]
    
    for i in range(len(ans)):
        if ans[i] in dtAnswer:
            dtAnswer[ans[i]].append(i)
        else:
            dtAnswer[ans[i]] = [i]
    
    for char in dtText:
        if char in dtAnswer:
            list1 = dtAnswer[char]
            list2 = dtText[char]
            list1.sort()
            list2.sort()
            for j in range(min(len(list1), len(list2))):
                score += abs(list1[j] - list2[j])
            score += max(0, abs(len(list1) - len(list2)))
        else:
            score += 2
    return score <= 30


print(compareTextWithAnswer("12344444", "1234444"))
```
