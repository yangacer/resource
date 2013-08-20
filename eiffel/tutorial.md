中文版譯自 http://tecomp.sourceforge.net/index.php?file=doc/lang/tutorial.txt

# 簡介

本文件提供一個快速的 Eiffel 簡介，涵蓋了撰寫 Eiffel 程式的基本需求。

就如著名的 "The C programming Language" 作者 Brian Kernighan 與 Denis Ritchie
所言，學習程式語言最好的方法就是用該語言寫程式。因此我們會專注在一些簡單但實用
的程式。大部分簡介中的程式為上述名著中範例的 Eiffel 版本。

以下並非程式設計簡介而是針對如何以 Eiffel 撰寫程式，因此會假設讀者已具備撰寫
C, C++, 或 java 的基礎能力。

# Hello world

我們的第一個程式僅印出 

```
  Hello, world
```

要達到這個目的的 Eiffel 程式如下

```
class
	HELLO
create 
	make
feature
	make
		do
			io.put_string("Hello, world")
			io.put_new_line
		end
end
```

所有 Eiffel 程式碼都存在於類別 (class) 之中。
各個類別的程式碼存在於單一檔案。以上的程式碼
必須被寫到一個 "hello.e" (全小寫) 的檔案中。大小寫對 Eiffel 來說是相同的。然而
類別名稱通常都是以大寫表示，而特徵 (feature) 通常是以小寫表示。

編譯與執行 Eiffel 程式依系統與編譯器不同。本文的資訊以 UNIX 環境下的 tecomp 為
準。為編譯該程式，編譯器需要額外資訊，這些資訊可透過 ace-file 取得。例如前面程式
的 ace-file "hello.ace" 可含有以下資訊

```
root
	HELLO.make
cluster
	"./"
	"`path_to_tecomp_installation'/library/kernel"
end
```

目前的 tecomp 版本需給定安裝目錄的絕對路徑或相對路徑，未來版本會支援符號定義方式。你可以鍵入以下命令來編譯與執行該程式

```
	tecomp hello.ace
```

然後它會印出

```
	Hello, world
```

接著來解釋一下這個程式。一個 Eiffel 程式由任意數量的類別組成。其中一個類別必須
是根類別 (root class) 、一個程序必須是根程序 (root procedure)。以上面的程式來說
根類別名為 HELLO 而跟程序為 make。

編譯器得知道如何找到類別；類別存在於通常以系統目錄實現的叢集 (cluster) 中。上面的
ace-file 定義了兩個叢集─ "./" (即當前目錄) 與 "`path_to_tecomp_installation'/library/kernel" (即 Eiffel 核心類別存放處。編譯器會在這些叢集中搜尋類別，當你的程式中
使用了一個搜尋不到的類別，編譯器則會提報錯誤。叢集中的 Eiffel 類別集合稱為 universe。

執行一個 Eiffel 程式從建立一個根型別的物件並呼叫其根程序開始 (此處型別與類別兩詞
為同義，它們只有在使用泛型時不同)。該跟程序可以創建任意數量的各種物件並呼叫任何已
建立物件的函式。

Eiffel 與許多現代語言相同，使用自由格式─程式語法中的空白字元是不重要的。退格
是為了人眼的可讀性而非編譯器限制。

名字像是 class, create, feature, do, 與 end 是語言中的關鍵字，它們是被保留的
；所有類別、特徵或變數不能使用這些保留字。

現在，我們來看看上面程式的結構

```
class
	HELLO	-- 類別名稱
create
	make	-- 建構程序
feature
	...	-- 類別特徵
end
```

這個框架表示一個 HELLO 類別的定義。型別為 HELLO 的物件只能以 make 建構程序建立。
所以特徵宣告在 feature...end 區塊。

一個特徵可以是函式或屬性。我們的簡單程式只有一個特徵稱為 make。該特徵為函式，
一個函式可以接受參數並回傳結果。沒有回傳結果的函式稱為命令 (command) 或程序
反之稱為查詢 (query)。make 這個函式為 HELLO 的建構程序是因為他被列在建構程序
的集合中(在 HELLO 中它是唯一一個)。

make 的程式碼

```
make
	do
		io.put_string("Hello, world")
		io.put_new_line
	end
```

只有兩個陳述式，```io.put_string("Hello, world")``` 呼叫 io 這個特徵；所有
Eiffel 中的類別都可以呼叫 io，因為該 io 為 STD_FILES 型別，此型別在 ANY 類別中
被定義而 ANY 是被所有的類別隱式繼承。既然 io 傳回的物件為 STD_FILES 型別
，STD_FILES 中的特徵得以被呼叫。

STL_FILES 有特徵 put_string 接受一個字串參數。該特徵 put_string 是一個命令因為
它沒有任何回傳值。它將字串參數輸出至標準輸出。而特徵 put_new_line 做的事情如字面
所述(譯：輸出換行)。

特徵的概念是 Eiffel 語言的基礎，因此以下解釋一些基本觀念：

一個特徵有兩種觀點─使用者或客戶端觀點，與實作觀點。由客戶端觀點，我們會區分查詢
與命令。一個查詢可接受零或多個參數並傳回一個值。雖然語言本身並未強迫，不過一個
查詢最好可以不含有副作用(譯：例如改變物件狀態，類似 C++ const member function 
概念)。一個命令一樣可以接受零或多個參數但沒有回傳值，它通常都被預期會改變物件
狀態。

因此，一個查詢可以屬性或函式實作，一個命令則需以程序實作。(?)

在兩個雙引號間的字串序列，像是 "Hello, world"，被稱為字串或字串常數。特殊字元如
換行或退格可藉由逸出序列包含在字串中。例如 %N 與 %T 為換行與退格的逸出序列。所以
我們也能這樣寫

```
io.put_string("Hello, world%N")
```

得到與下面程式同樣的輸出

```
io.put_string ("Hello, world")
io.put_new_line
```

果想要依照程式碼中的格式輸出含有多個換行的字串，可以使用 verbatim 字串，例如

```
io.put_string ("[
  	usage: tecomp options ace_file
 
		options
			-t{p,v,e}{0,1,2,3}	trace parsing, validation,
						execution with level 0,1,2,3
			-ws{0,1,2,3}		write statistics
  		 ]")
```

會完全依照程式碼中字串的格式做輸出(譯：保留所有空白字元)。

將多行 vertatim 字串擺在 "[" 與 "]" 之間會移除每一行的"最長相同前綴空白字元
(longest common whitespace prefix)" (即向左對齊)。若擺在 "{" 與 "}" 間則會直接
複製所有字串而不移除任何前綴。Verbatim 字串常數與此文件語法和許多 UNIX shell 
類似。

若想在原碼中的字串換行但不希望換行包含在字串時，可使用包裝字串。陳述式

```
io_put_string ("Hello, %
		%world")
```

與下面有相同輸出。

```
io.put_string("Hello, world%N")
```

在兩個 '%' 符號中的空白字元會在建構字串常數的過程中被忽略。

ANY 類別還有另一個特徵 print 用來將任何物件輸出到標準輸出。所以最短的 Hello world程式長得像這樣

```
class HELLO create make feature
	make 
		do
			print("Hello, world%N")
		end
end
```


# 區域變數、計算式與迴圈

# 字元輸入與輸出

## 檔案複製

## 字數計算

# 陣列與物件

# 函式

# 更專注於類別

## 一個矩形是一種形狀

## 矩陣物件

## 複數

# 連結串列


# 中英對照

```
類別 class
物件 object
程序 procedure
特徵 feature
逸出序列 escaped sequence
屬性 attribute
```
