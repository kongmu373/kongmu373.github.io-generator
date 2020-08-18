---
title: "29 Employee Importance"
date: 2020-07-23T09:07:45+08:00
draft: true
series: ["easy"]
categories: ["leetcode"]
---


## leetcode-29-employee-importance

```java
/*
// Definition for Employee.
class Employee {
    public int id;
    public int importance;
    public List<Integer> subordinates;
};
*/

class Solution {
        public int getImportance(List<Employee> employees, int id) {
            // 1. DFS
            HashMap<Integer, Employee> es = new HashMap<>();
            for (Employee employee : employees) {
                es.put(employee.id, employee);
            }
            LinkedList<Integer> q = new LinkedList<>();
            q.push(id);
            int sum = 0;
            // 非递归
            while (!q.isEmpty()) {
                int eid = q.pop();
                Employee e = es.get(eid);
                sum += e.importance;
                for (Integer subordinate : e.subordinates) {
                    q.push(subordinate);
                }
            }
            return sum;
            
            // return dfs(id, es);
        }

        // dfs 递归版
        private int dfs(int id, HashMap<Integer, Employee> es) {
            Employee e = es.get(id);
            if(e == null) return 0;
            int sum = e.importance;
            for (Integer subordinate : e.subordinates) {
                sum += dfs(subordinate, es);
            }
            return sum;
        }
}
```
