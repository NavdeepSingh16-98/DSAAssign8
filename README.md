# DSAAssign8

# Q1

```
function minimumDeleteSum(s1, s2) {
    const m = s1.length;
    const n = s2.length;
    
    const dp = Array(m + 1).fill(0).map(() => Array(n + 1).fill(0));
    
    for (let i = m - 1; i >= 0; i--) {
        dp[i][n] = dp[i + 1][n] + s1.charCodeAt(i);
    }
    
    for (let j = n - 1; j >= 0; j--) {
        dp[m][j] = dp[m][j + 1] + s2.charCodeAt(j);
    }
    
    for (let i = m - 1; i >= 0; i--) {
        for (let j = n - 1; j >= 0; j--) {
            if (s1[i] === s2[j]) {
                dp[i][j] = dp[i + 1][j + 1];
            } else {
                dp[i][j] = Math.min(dp[i + 1][j] + s1.charCodeAt(i), dp[i][j + 1] + s2.charCodeAt(j));
            }
        }
    }
    
    return dp[0][0];
}

```

# Q2

```
function isValid(s) {
    const stack = [];
    let wildcardCount = 0;
    
    for (let i = 0; i < s.length; i++) {
        const char = s[i];
        
        if (char === "(") {
            stack.push(char);
        } else if (char === ")") {
            if (stack.length === 0) {
                if (wildcardCount === 0) {
                    return false;
                }
                
                wildcardCount--;
            } else {
                stack.pop();
            }
        } else {
            wildcardCount++;
        }
    }
    
    while (stack.length > 0) {
        if (wildcardCount === 0) {
            return false;
        }
        
        stack.pop();
        wildcardCount--;
    }
    
    return true;
}

```

# Q3

```
function minDistance(word1, word2) {
    const m = word1.length;
    const n = word2.length;
    
    const dp = Array(m + 1).fill(0).map(() => Array(n + 1).fill(0));
    
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    
    const lcsLength = dp[m][n];
    
    return m + n - 2 * lcsLength;
}

```

# Q4

```
class TreeNode {
    constructor(val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}

function buildTree(s) {
    if (s.length === 0) {
        return null;
    }
    
    const root = new TreeNode(parseInt(s));
    const stack = [root];
    let i = 1;
    
    while (stack.length > 0 && i < s.length) {
        const node = stack.pop();
        let left = null;
        let right = null;
        
        if (s[i] === "(") {
            let j = i + 1;
            let parenthesesCount = 1;
            
            while (parenthesesCount > 0) {
                if (s[j] === "(") {
                    parenthesesCount++;
                } else if (s[j] === ")") {
                    parenthesesCount--;
                }
                
                j++;
            }
            
            left = s.substring(i + 1, j - 1);
            i = j;
        }
        
        if (s[i] === "(") {
            let j = i + 1;
            let parenthesesCount = 1;
            
            while (parenthesesCount > 0) {
                if (s[j] === "(") {
                    parenthesesCount++;
                } else if (s[j] === ")") {
                    parenthesesCount--;
                }
                
                j++;
            }
            
            right = s.substring(i + 1, j - 1);
            i = j;
        }
        
        if (left !== null) {
            node.left = new TreeNode(parseInt(left));
            stack.push(node.left);
        }
        
        if (right !== null) {
            node.right = new TreeNode(parseInt(right));
            stack.push(node.right);
        }
        
        i++;
    }
    
    return root;
}

```

# Q5

```

function compress(chars) {
    let index = 0;
    let i = 0;
    
    while (i < chars.length) {
        let j = i + 1;
        
        while (j < chars.length && chars[j] === chars[i]) {
            j++;
        }
        
        const count = j - i;
        
        chars[index] = chars[i];
        index++;
        
        if (count > 1) {
            const countString = count.toString();
            
            for (let k = 0; k < countString.length; k++) {
                chars[index] = countString[k];
                index++;
            }
        }
        
        i = j;
    }
    
    return index;
}

```

# Q6

```
function findAnagrams(s, p) {
    const result = [];
    const pFreq = Array(26).fill(0);
    const sFreq = Array(26).fill(0);
    const aCode = "a".charCodeAt(0);
    
    for (let i = 0; i < p.length; i++) {
        pFreq[p.charCodeAt(i) - aCode]++;
        sFreq[s.charCodeAt(i) - aCode]++;
    }
    
    for (let i = p.length; i < s.length; i++) {
        if (arraysEqual(pFreq, sFreq)) {
            result.push(i - p.length);
        }
        
        sFreq[s.charCodeAt(i) - aCode]++;
        sFreq[s.charCodeAt(i - p.length) - aCode]--;
    }
    
    if (arraysEqual(pFreq, sFreq)) {
        result.push(s.length - p.length);
    }
    
    return result;
}

function arraysEqual(arr1, arr2) {
    if (arr1.length !== arr2.length) {
        return false;
    }
    
    for (let i = 0; i < arr1.length; i++) {
        if (arr1[i] !== arr2[i]) {
            return false;
        }
    }
    
    return true;
}

```

# Q7

```
function decodeString(s) {
    const stack = [];
    
    for (let i = 0; i < s.length; i++) {
        if (s[i] !== "]") {
            stack.push(s[i]);
        } else {
            let str = "";
            
            while (stack.length > 0 && stack[stack.length - 1] !== "[") {
                str = stack.pop() + str;
            }
            
            stack.pop(); // Pop the opening bracket
            
            let num = "";
            
            while (stack.length > 0 && !isNaN(stack[stack.length - 1])) {
                num = stack.pop() + num;
            }
            
            num = parseInt(num);
            
            let repeatedStr = "";
            
            for (let j = 0; j < num; j++) {
                repeatedStr += str;
            }
            
            stack.push(repeatedStr);
        }
    }
    
    return stack.join("");
}

```

# Q8

```
function buddyStrings(s, goal) {
    if (s.length !== goal.length) {
        return false;
    }
    
    if (s === goal) {
        const charCounts = Array(26).fill(0);
        
        for (let i = 0; i < s.length; i++) {
            const charCode = s.charCodeAt(i) - "a".charCodeAt(0);
            charCounts[charCode]++;
            
            if (charCounts[charCode] > 1) {
                return true;
            }
        }
        
        return false;
    }
    
    const mismatches = [];
    
    for (let i = 0; i < s.length; i++) {
        if (s[i] !== goal[i]) {
            mismatches.push(i);
        }
    }
    
    if (mismatches.length === 2) {
        const [m1, m2] = mismatches;
        
        if (s[m1] === goal[m2] && s[m2] === goal[m1]) {
            return true;
        }
    }
    
    return false;
}

```

