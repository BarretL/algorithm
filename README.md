## 【JavaScript】算法练习

### 项目说明

> 本项目记录个人平时练习各类型的算法题解题思路及解题源码，希望对各位有所帮助

### 目录结构

```
|- src                            // 资源文件夹
|- |- 二叉树相关算法
|- |- 基础算法题库
|- |- |- 猜数字游戏.js
|- |- |- 单词的压缩编码.js
|- |- |- 反转字符串中的单词.js
|- |- |- 反转字符串中的单词2.js
|- |- 排序算法
|- |- |- 插入排序.js
|- |- |- 堆排序.js
|- |- |- 归并排序.js
|- |- |- 快速排序.js
|- |- |- 冒泡排序.js
|- |- |- 选择排序.js
|- LICENSE                        // MIT 许可
|- README.md                      // 项目介绍
```

#### 排序算法

- 冒泡排序

  ```
  /**
   * 时间复杂度O(n*n)
   * 算法思路：分两层循环。
   *      1、外层循环 n 次；
   *      2、内层循环 n - i 次，相邻元素比较，前者大于后者则互换位置
   */
  function bubbleSort(testDataArr) {
    // 两层for循环
    for (let i = 1; i < testDataArr.length; i++) {
      // 内层比较内层比较(相邻元素大者靠后)
      for (let j = 0; j < testDataArr.length - i; j++) {
        if (testDataArr[j] > testDataArr[j + 1]) {
          [testDataArr[j], testDataArr[j + 1]] = [testDataArr[j + 1], testDataArr[j]];
        }
      }
    }
  }
  ```
#### 基础算法题库

- 猜数字游戏
> 思路：
>> 方法一：第一次 `for` 循环统计公牛数，第二次 `for` 循环统计奶牛数
>>
>> 方法二：第一次 `forEach` 统计公牛数，并将剩余的非公牛数的数字以对象属性的方式记录出现的次数，将非公牛的 `guess` 存入 `restGuessArray` 中；第二次 `forEach` 根据 `restGuessArray` 当中出现的值去 `numberObj` 对象当中看是否有对应的属性，若有，奶牛计数减一，对应的属性值计数减一

  ```
  /**
   * 你正在和你的朋友玩 猜数字（Bulls and Cows）游戏：你写下一个数字让你的朋友猜。每次他猜测后，你给他一个提示，
   * 告诉他有多少位数字和确切位置都猜对了（称为“Bulls”, 公牛），有多少位数字猜对了但是位置不对（称为“Cows”, 奶牛）。
   * 你的朋友将会根据提示继续猜，直到猜出秘密数字。
   *
   * 请写出一个根据秘密数字和朋友的猜测数返回提示的函数，用 A 表示公牛，用 B 表示奶牛。
   *
   * 请注意秘密数字和朋友的猜测数都可能含有重复数字。
   *
   * 示例 1:
   *
   * 输入: secret = "1807", guess = "7810"
   *
   * 输出: "1A3B"
   *
   * 解释: 1 公牛和 3 奶牛。公牛是 8，奶牛是 0, 1 和 7。
   * 示例 2:
   *
   * 输入: secret = "1123", guess = "0111"
   *
   * 输出: "1A1B"
   *
   * 解释: 朋友猜测数中的第一个 1 是公牛，第二个或第三个 1 可被视为奶牛。
   * 说明: 你可以假设秘密数字和朋友的猜测数都只包含数字，并且它们的长度永远相等。
   */

  /**
   * 方法一：两次for循环，分别找出公牛和奶牛
   */

  /**
   * @param {string} secret
   * @param {string} guess
   * @return {string}
   */
  var getHint = function(secret, guess) {
    let bulls = [],
      cows = [];
    let secretArray = secret.split("");
    let guessArray = guess.split("");
    secretArray.forEach((item, index) => {
      // 找出所有的公牛
      if (item === guessArray[index]) {
        bulls.push(item);
        secretArray[index] = guessArray[index] = "A";
      }
    });
    secretArray.forEach((item, index) => {
      if (item !== "A" && guessArray.includes(item)) {
        // 找出所有的奶牛
        cows.push(item);
        guessArray[guessArray.indexOf(item)] = "B";
      }
    });
    return `${bulls.length}A${cows.length}B`;
  };

  /**
   * 方法二：先找出公牛，并记录guess数据，利用obj计数
   */

  /**
   * @param {string} secret
   * @param {string} guess
   * @return {string}
  */
  var getHint = function(secret, guess) {
    let bullsCount = 0,
      cowsCount = 0,
      numberObj = {},
      restGuessArray = [];
    let secretArray = secret.split("");
    let guessArray = guess.split("");
    secretArray.forEach((item, index) => {
      if (item === guessArray[index]) {
        // 公牛数加1
        bullsCount++;
      } else {
        // 将 secret 不是公牛的数字通过 对象属性 的方式记录出现的次数
        numberObj[item] = (numberObj[item] || 0) + 1;
        // 取出猜测的数据
        restGuessArray.push(guessArray[index]);
      }
    });
    restGuessArray.forEach(item => {
      if (numberObj[item] > 0) {
        cowsCount++;
        numberObj[item]--;
      }
    });
    return `${bullsCount}A${cowsCount}B`;
  };

  ```

- 单词的压缩编码
> 思路：将输入的数组 `每个值` 转化为 `对象属性` 和 `属性值` ，遍历对象，将每项属性值通过 `substring` 的方式，截取每个子串，删除对象内属性名为子串值的属性，最后留下来的对象属性就是我们需要压缩之后的每个字符串，最后遍历对象计算所有属性值的长度和即可 `resultLen = sum(every(attr.length + 1))`

  ```
    /**
     * 给定一个单词列表，我们将这个列表编码成一个索引字符串 S 与一个索引列表 A。
     * 例如，如果这个列表是 ["time", "me", "bell"]，我们就可以将其表示为 S = "time#bell#" 和 indexes = [0, 2, 5]。
     * 对于每一个索引，我们可以通过从字符串 S 中索引的位置开始读取字符串，直到 "#" 结束，来恢复我们之前的单词列表。
     * 那么成功对给定单词列表进行编码的最小字符串长度是多少呢？
     */

    /**
     * 示例：
     * 输入: words = ["time", "me", "bell"]
     * 输出: 10
     * 说明: S = "time#bell#" ， indexes = [0, 2, 5] 。
     *
     * 提示：
     * 1 <= words.length <= 2000
     * 1 <= words[i].length <= 7
     * 每个单词都是小写字母 。
     */

    /**
     * @param {string[]} words
     * @return {number}
     */
    var minimumLengthEncoding = function(words) {
      let obj = {},
        len = 0;
      for (let index = 0; index < words.length; index++) {
        const element = words[index];
        obj[element] = element;
      }

      for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
          const element = obj[key];
          for (let index = 1; index < element.length; index++) {
            const item = element.substring(index);
            delete obj[item];
          }
        }
      }

      for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
          const element = obj[key];
          len += element.length + 1;
        }
      }
      return len;
    };

    console.log(minimumLengthEncoding(["t"]));
    console.log(minimumLengthEncoding(["time", "me", "bell"]));
    console.log(minimumLengthEncoding(["time", "time", "time"]));
  ```

- 反转字符串中的单词
> 思路：
>> 方法一：把字符串按照分割成数组，遍历每一项倒叙即可
>>
>> 方法二：
>>     1、把字符串按照每个字符拆分成数组，整体 `reverse`，然后 `join` 合并单词
>>     2、按照 `" "` 空格字符拆分为数组，然后 `reverse`, 然后 `join` 合并为字符串

  ```
  /**
   * 给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。
   *
   * 示例 1:
   *
   * 输入: "Let's take LeetCode contest"
   * 输出: "s'teL ekat edoCteeL tsetnoc"
   * 注意：在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。
   */

  /**
   * 方法一：把字符串按照分割成数组，遍历每一项倒叙即可
   */

  /**
   * @param {string} s
   * @return {string}
   */
  var reverseWords = function(s) {
    let sArr = s.split(" ");
    sArr.forEach((item, index) => {
      sArr[index] = item
        .split("")
        .reverse()
        .join("");
    });

    return sArr.join(" ");
  };

  /**
   * 方法二：
   *    1、把字符串按照每个字符拆分成数组，整体 `reverse`，然后 `join` 合并单词
   *    2、按照 `" "` 空格字符拆分为数组，然后 `reverse`, 然后 `join` 合并为字符串
   */

  /**
   * @param {string} s
   * @return {string}
   */
  var reverseWords = function(s) {
    let arr = s
      .split("")
      .reverse()
      .join("");
    return arr
      .split(" ")
      .reverse()
      .join(" ");
  };

  console.log(reverseWords("Let's take LeetCode contest"));
  ```

- 反转字符串中的单词2
> 思路：
```
  将字符串按字母拆分，转化为数组。
  分情况讨论：
  1、字符串长度 < k ，直接反转返回即可
  2、遍历数组 for (let i = 0; i < sArr.length; )
    a) i 以 2 * k 递增
    b) 反转 [i, i + k) 的数据
    c) 若剩余数据长度小于 k (sArr.length - i < k)，则直接反转
    d) 若剩余数据长度大于 k ，小于 2k，则将 [k, sArr.length) 的数据拼接在 result 之后即可
```

  ```
    /**
     * 给定一个字符串和一个整数 k，你需要对从字符串开头算起的每个 2k 个字符的前k个字符进行反转。
     * 如果剩余少于 k 个字符，则将剩余的所有全部反转。
     * 如果有小于 2k 但大于或等于 k 个字符，则反转前 k 个字符，并将剩余的字符保持原样。
     *
     * 示例:
     *
     * 输入: s = "abcdefg", k = 2
     * 输出: "bacdfeg"
     * 要求:
     *
     * 该字符串只包含小写的英文字母。
     * 给定字符串的长度和 k 在[1, 10000]范围内。
     */

    /**
     * @param {string} s
     * @param {number} k
     * @return {string}
     */
    var reverseStr = function(s, k) {
      let result = "";
      if (s.length <= k) {
        return s
          .split("")
          .reverse()
          .join("");
      } else {
        let sArr = s.split("");
        for (let i = 0; i < sArr.length; ) {
          if (sArr.length - i < k) {
            result += sArr
              .slice(i, sArr.length)
              .reverse()
              .join("");
          } else {
            result += sArr
              .slice(i, i + k)
              .reverse()
              .join("");
            if (i + 2 * k > sArr.length) {
              result += sArr.slice(i + k, sArr.length).join("");
            } else {
              result += sArr.slice(i + k, i + 2 * k).join("");
            }
          }
          i += 2 * k;
        }
      }
      return result;
    };

    console.log(
      reverseStr(
        "hyzqyljrnigxvdtneasepfahmtyhlohwxmkqcdfehybknvdmfrfvtbsovjbdhevlfxpdaovjgunjqlimjkfnqcqnajmebeddqsgl",
        39
      )
    );

  ```

## 说明

+ 部分算法题目来源：LeetCode（力扣）题库
+ 部分算法题目来源：牛客网
+ 部分算法题目来源：百度