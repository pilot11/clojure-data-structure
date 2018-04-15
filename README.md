# clojure数据结构

## vector
vector是一种顺序数据结构,比较高效的随机访问。类似于java的ArrayList。
	
* vector以中括号来表示：
  ```clojure
  ["a" "b" "c" "d"] 
  => ["a" "b" "c" "d"]

  (vector "a" "b" "c" "d") 
  => ["a" "b" "c" "d"]

  ; vector中的元素无需拥有相同的数据类型：
  ["a" 1 ["a" "b"] inc] 
  => ["a" 1 ["a" "b"] #object[clojure.core$inc ...]]
  ```
* 元素个数
  ```clojure
  (count [1 2 3])
  => 3
	```
* 作为有序数据结构，支持使用下标来访问：
  ```clojure
  (nth ["a" "b" "c" "d"] 1)
  => "b"

  (get ["a" "b" "c" "d"] 1)
  => "b"

  (["a" "b" "c" "d"] 1)
  => "b"
  ```		
* 还有一些特别的便捷访问：
  ```clojure
  ; 第一个元素
  (first ["a" "b" "c" "d"])
  => "a"

  ; 第一个元素中的第一个元素
  (ffirst [["a" "b"] "c" "d"])
  => "a"

  ; 第二个元素
  (second ["a" "b" "c" "d"])
  => "b"

  ; 最后一个元素
  (last ["a" "b" "c" "d"])
  => "d"

  ; 第一个以外的元素
  (rest ["a" "b" "c" "d"])
  => ("b" "c" "d")
  ```
* 增加一个元素
  ```clojure
  ; 有序数据结构，增加的数据放在最后
  (conj ["a" "b" "c" "d"] "e")
  => ["a" "b" "c" "d" "e"]
  ```		
* 修改一个元素
  ```clojure
  ; 更改指定下标的元素(更换值)
  (assoc ["a" "b" "c" "d"] 1 "e")
  => ["a" "e" "c" "d"]

  ; 更改指定下标的元素(操作)
  (update [4 5 6 7] 1 inc)
  => [4 6 6 7]
  ```
* 删除
  ```clojure
  (remove #(< % 7) [5 6 7 8])
  => (7 8)
  ```		
* 选择
  ```clojure
  ; 选择前面两个
  (take 2 ["a" "b" "c" "d"])
  => ("a" "b")

  ; 选择最后两个
  (take-last 2 ["a" "b" "c" "d"])
  => ("c" "d")

  ; 从第一个开始选择符合条件的元素
  (take-while #(< % 7) [5 6 7 8 5])
  => (5 6)

  ; 选择所有符合条件的元素
  (filter #(< % 7) [5 6 7 8 5])
  => (5 6 5)
  ```
* 遍历
  ```clojure
  ; 遍历,对每个元素自增1
  (map inc [5 6 7 8])
  => (6 7 8 9)
  (mapv inc [5 6 7 8])
  => [6 7 8 9]
  (vec (map inc [5 6 7 8]))
  => [6 7 8 9]

  (for [x [5 6 7 8]]
    (inc x))
  => (6 7 8 9)
  ```
## list
list是单向列表，插入、删除数据比较高效,随机访问的效率比vector要弱。操作方法上与vector十分相似。

* list以小括号来表示,符号 ' 表示不要对其进行求值：
  ```clojure
  '("a" "b" "c" "d") 
  => ("a" "b" "c" "d") 

  (list "a" "b" "c" "d") 
  => ("a" "b" "c" "d")
  ```		
* 函数、符号的定义也是用小括号（没错，函数就是数据，是list数据,第一个元素是执行函数，后面的元素是该函数的参数）
  ```clojure
  (def PI 3.14159)

  (defn plus
    [x y]
    (+ x y))
  ```
* 元素个数
  ```clojure
  (count '(1 2 3))
  => 3
  ```		
* 使用下标来访问：
  ```clojure
  (nth '("a" "b" "c" "d") 1)
  => "b"

  (get (list "a" "b" "c" "d") 1)
  => nil  ; 与vector不同

  ('("a" "b" "c" "d") 1)
  Exception   ; 与vector不同
  ```		
* 便捷访问：
  ```clojure
  ; 第一个元素
  (first '("a" "b" "c" "d"))
  => "a"

  ; 第一个元素中的第一个元素
  (ffirst '(("a" "b") "c" "d"))
  => "a"

  ; 第二个元素
  (second '("a" "b" "c" "d"))
  => "b"

  ; 最后一个元素
  (last '("a" "b" "c" "d"))
  => "d" 

  ; 第一个以外的元素
  (rest '("a" "b" "c" "d"))
  => ("b" "c" "d")
  ```
* 增加一个元素
  ```clojure
  ; 有序数据结构，增加的数据放在开头（与vector不同）
  (conj '("a" "b" "c" "d") "e")
  => ("e" "a" "b" "c" "d")
  ```		
* 修改一个元素
  ```clojure
  ; 更改指定下标的元素(更换值)
  (assoc '("a" "b" "c" "d") 1 "e")
  Exception   ; 与vector不同

  ; 更改指定下标的元素(操作)
  (update '(4 5 6 7) 1 inc)
  Exception   ; 与vector不同
  ```
* 删除
  ```clojure
  (remove #(< % 7) '(5 6 7 8))
  => (7 8)
  ```		
* 选择
  ```clojure
  ; 选择前面两个
  (take 2 '("a" "b" "c" "d"))
  => ("a" "b")

  ; 选择最后两个
  (take-last 2 '("a" "b" "c" "d"))
  => ("c" "d")

  ; 从第一个开始选择符合条件的元素
  (take-while #(< % 7) '(5 6 7 8 5))
  => (5 6)

  ; 选择所有符合条件的元素
  (filter #(< % 7) '(5 6 7 8 5))
  => (5 6 5)
  ```
* 遍历
  ```clojure
  ; 遍历,对每个元素自增1
  (map inc '(5 6 7 8))
  => (6 7 8 9)

  (for [x '(5 6 7 8)]
    (inc x))
  => (6 7 8 9)
  ```
## set

set是数据集合，所有元素都是**唯一**的，是无序的数据结构。

* set以#{}来表示：
  ```clojure
  #{"a" "b" "c" "d"}
  => #{"a" "b" "c" "d"}

  (hash-set "a" "b" "c" "d")
  => #{"d" "a" "b" "c"}

  (set ["a" "b" "c" "d"])
  => #{"d" "a" "b" "c"}
  ```
* 元素个数：
  ```clojure
  (count #{"a" "b" "c" "d"})
  => 4
  ```		
* 是否包含某个元素：
  ```clojure
  (get #{"d" "a" "b" "c"} "a")
  => "a"

  (contains? #{"d" "a" "b" "c"} "a")
  => true

  (#{"d" "a" "b" "c"} "a")
  => "a"
  ```
* 增加一个元素
  ```clojure
  (conj #{"d" "a" "b" "c"} "e")
  => #{"d" "e" "a" "b" "c"}
  ```
* 删除
  ```clojure
  (disj #{"d" "a" "b" "c"} "c")
  => #{"d" "a" "b"}
  ```
* 遍历
  ```clojure
  ; 遍历,对每个元素自增1
  (map inc #{5 6 7 8})
  => (6 7 8 9)

  (for [x #{5 6 7 8}]
    (inc x))
  => (6 7 8 9)
  ```		
## map
map是把key和value关联起来的数据结构，map中的key是唯一的,它的元素是 k-v 对。value可以是任意类型的数据，包括vector、list、map、函数等。key也可以是任意类型的数据，但从使用方便上来考虑，多用keyword和string类型。map是clojure中最灵活、常用的数据结构。

* map以花括号来表示, keyword类型数据以 : 来表示：
  ```clojure
  {:a 1 :b 2}
  => {:a 1 :b 2} 

  {"a" 1 "b" 2}
  => {"a" 1 "b" 2}

  {["a" "b"] 1 inc 2}
  => {["a" "b"] 1, #object[clojure.core$inc ..."] 2}
  ```
* 元素个数（k-v对的个数）
  ```clojure
  (count {"a" 1 "b" 2})
  => 2
  ```		
* map中所有的key或value
  ```clojure
  (keys {:a 1 :b 2 :c 3 :d 4})
  => (:a :b :c :d)

  (vals {:a 1 :b 2 :c 3 :d 4})
  => (1 2 3 4)
  ```
* 是否包含某个key：
  ```clojure
  (contains? {:a 1 :b nil} :b)
  => true

  (get {:a 1 :b false} :b)
  => false

  (get {:a 1 :b nil} :b)
  => nil

  (get {:a 1 :b nil} :c)
  => nil
  ```		
* 使用key来访问value：
  ```clojure
  (get {"a" 1 "b" 2} "a")
  => 1

  ; 可以指定缺省值
  (get {"a" 1 "b" 2} "c" 3)
  => 3

  (get {:a 1 :b 2} :a)
  => 1

  ; map数据可以作为函数,并且可以指定缺省值
  ({"a" 1 "b" 2} "a")
  => 1
  ({"a" 1 "b" 2} "c" 3)
  => 3

  ({:a 1 :b 2} :a)
  => 1
  ({:a 1 :b 2} :c 3)
  => 3

  ; keyword类型数据可以作为函数，但string类型不行
  (:a {:a 1 :b 2})
  => 1
  (:c {:a 1 :b 2} 3)
  => 3

  ("a" {"a" 1 "b" 2})
  Exception 

  ; 嵌套map的访问
  (def nest-map
    {:a 1
     :b {:c 2
         :d {:e 3
             :f {:g 4}}}})

  (:g (:f (:d (:b nest-map))))
  => 4
  ((((nest-map :b) :d) :f) :g)
  => 4       ; 传说中亮瞎眼的括号

  (get-in nest-map [:b :d :f :g])
  => 4	; 应该用路径来访问

  (assoc-in nest-map [:b :d :f :g] "g")
  => {:a 1, :b {:c 2, :d {:e 3, :f {:g "g"}}}} ; 更换值

  (update-in nest-map [:b :d :f :g] inc)
  => {:a 1, :b {:c 2, :d {:e 3, :f {:g 5}}}}  ; 操作值
  ```
* 有序数据方式的访问（很少用）：
  ```clojure
  ; 第一个元素
  (first {:a 1 :b 2})
  => [:a 1]

  ; 第一个元素中的第一个元素
  (ffirst {:a 1 :b 2})
  => :a

  ; 第二个元素
  (second {:a 1 :b 2}})
  => [:b 2]

  ; 第一个以外的元素
  (rest {:a 1 :b 2 :c 3})
  => ([:b 2] [:c 3])
  ```
* 增加k-v对（如果k已存在，则替换）
  ```clojure
  (assoc {:a 1} :b 2)
  => {:a 1, :b 2}

  (assoc {:a 1} :a 2)
  => {:a 2}

  (conj {:a 1} [:b 2])
  => {:a 1, :b 2} ; 一般不对map使用conj

  ; 加几个k-v对
  (assoc {:a 1} :b 2 :c 3 :d 4)
  => {:a 1, :b 2, :c 3, :d 4}

  (merge {:a 1} {:b 2 :c 3 :d 4})
  => {:a 1, :b 2, :c 3, :d 4}
  (merge {:a 1} {:a 2 :b 2 :c 3 :d 4})
  => {:a 2, :b 2, :c 3, :d 4} ; 第二个map的key覆盖第一个map中相同的key
  ```		
* 修改元素
  ```clojure
  (assoc {:a 1} :a 2)
  => {:a 2}

  (assoc-in {:a 1 :b {:c 1}} [:b :c] 2)
  => {:a 1, :b {:c 2}}

  (update {:a 1} :a inc)
  => {:a 2}

  (update-in {:a 1 :b {:c 1}} [:b :c] inc)
  => {:a 1, :b {:c 2}}
  ```
* 删除
  ```clojure
  (dissoc {:a 1 :b 2} :b)
  => {:a 1}

  ; 没有提供专门删除嵌套map中key的函数，而是把这个任务交给更新函数来处理
  (update {:a 1 :b {:c 1}} :b dissoc :c)
  => {:a 1, :b {}}

  (update-in {:a 1
              :b {:c 2
                  :d {:e 3
                      :f {:g 4}}}}
             [:b :d :f]
             dissoc :g)
  => {:a 1, :b {:c 2, :d {:e 3, :f {}}}}
  ```
* 选择
  ```clojure
  (select-keys {:a 1 :b 2 :c 3 :d 4} [:b :c])
  => {:b 2, :c 3}

  ; 类似于vector、list的方式
  (take 2 {:a 1 :b 2})
  => ([:a 1] [:b 2])
  ; 返回内容是kv对
  ; 其他 take-last、take-while、filter等函数结果也都类似，一般都不用
  ```
* 遍历
  ```clojure
  ; 遍历,遍历的是k-v对
  (map identity {:a 1 :b 2 :c 3 :d 4})
  => ([:a 1] [:b 2] [:c 3] [:d 4]) ; identity是一个只有一个参数的函数，返回值就是这个输入参数

  ; 处理完的结果保持map类型
  (into {}
        (map identity {:a 1 :b 2 :c 3 :d 4}))
  => {:a 1, :b 2, :c 3, :d 4}

  (into {}
        (map (fn [[k v]] [(name k) (inc v)]) {:a 1 :b 2 :c 3 :d 4}))
  => {"a" 2, "b" 3, "c" 4, "d" 5}

  ; for 也一样适用
  (for [[k v] {:a 1 :b 2 :c 3 :d 4}]
    [(name k) (inc v)])
  => (["a" 2] ["b" 3] ["c" 4] ["d" 5])
  ```			
## lazy-seq

序列seq,是获取和遍历collection类型数据（vector、list、map、set都是collection抽象的实现）的一种顺序视图。核心的函数如上文提到的first、second、rest、nth、take等。lazy-seq惰性序列，是clojure上很有意思的一种数据结构，这个序列上的元素，只有在需要的时候才求值。类似于java/C++中逻辑判断表达式一样，不用到的部分不会被计算。
  ```java
  // java/C++中,如果下面表达式中的 b > 2 是false,表达式 c > 3 不会被用到，就不用再对其求值
  if (a > 1 && b > 2 && c > 3) ...
  ```
这个特性，允许clojure表达出无穷序列，比如自然数序列 1、2、3 ...
  ```clojure
  ; 无限自然数序列（别在repl中求值，会撑爆你的内存或者长整形数的，它真的是无穷的）
  (iterate inc 1)

  ; 取前面几个，用多少个就计算多少个
  (take 5 (iterate inc 1))
  => (1 2 3 4 5)

  ; 取第几个（注意下标是从0开始的）
  (nth (iterate inc 1) 5)
  => 6
  ```
  
一个阶乘序列：
  ```clojure
  ; 计算乘下一个数
  (defn next-factorial
    [[index fact]]
    [(inc index) (* fact (inc index))])

  ; 无限阶乘序列，（别着急在REPL中执行）
  (iterate next-factorial [1 1]) ;每个元素是乘数与阶乘值的vector
  (map second (iterate next-factorial [1 1])) ;每个元素是阶乘值

  ; 取几个来看看
  (take 5 (iterate next-factorial [1 1]))
  => ([1 1] [2 2] [3 6] [4 24] [5 120])

  ; 只要阶乘结果，不要乘数
  (take 5 (map second (iterate next-factorial [1 1])))
  => (1 2 6 24 120)

  ; 指定第几个(不要忘记下标是从0开始的)
  (nth (map second (iterate next-factorial [1 1])) 4)
  => 120
  ```

## License

* copyright 深圳市风林火山电脑技术有限公司
* author  陈建业
* email pilot11@163.com
