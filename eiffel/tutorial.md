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

目前的 tecomp 版本需給定安裝目錄的絕對路徑或相對路徑，未來版本會支援符號定義方式。你可以
鍵入以下命令來編譯與執行該程式

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
ace-file 定義了兩個叢集─ "./" (即當前目錄) 與 "`path_to_tecomp_installation'/library/kernel" 
(即 Eiffel 核心類別存放處。編譯器會在這些叢集中搜尋類別，當你的程式中
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

下一個程式使用公式

```
degree Celsius = (5/9)( degree Fahrenheit - 32 )
```

印出以下的華氏攝氏溫度對照表：

```
	0       -17
	20      -6
	40      4
	60      15
	80      26
	100     37
	120     48
	140     60
	160     71
	180     82
	200     93
	220     104
	240     115
	260     126
	280     137
	300     148
```

此表可以下面的 Eiffel 程式輸出

```
class
	FAHR_CELSIUS 
		-- 印出華氏-攝氏對照表
create
	make
feature
	make
		local
			fahr: INTEGER -- 華氏溫度
		do
			from
				fahr := 0
			until
				fahr > 300
			loop
				io.put_character('%T')
				io.put_integer	(fahr)
				io.put_character('%T')
				io.put_integer  ((fahr-32) * 5 // 9)
				io.put_new_line
				fahr := fahr + 20
			end
		end
end
```

任何在 '--' 與行尾間的字元都會被編譯器忽略，這些字元可作為註解。

Eiffel 中，區域變數可在各個函式中的 do end 區塊前宣告。上面的程式中，區域變數
fahr 宣告為 INTEGER 型別；Eiffel 是強型別語言，因此任何變數，運算式等都必須
對應一個型別。 在 Eiffel 裡，一個 INTEGER 是介於 -2^31 至 2^31 - 1 之間的數值
，意即 INTEGER 至少有 32 位元。

INTEGER 為核心函式庫中的一個類別。

FAHR_CELSIUS 的程序 _make_  含有一迴圈，由初始化區段、終止條件、與迴圈本體組成。
在 Eiffel 裡一個迴圈運作如下。

- 執行初始化區段 (from ...)

- 測試終止條件 (until ...)

- 當條件為假，迴圈本體 (loop ...) 即被執行，之後再次測試終止條件

- 一旦終止條件為真，迴圈終止並執行迴圈(... end)之後的第一個陳述式

運算式 ```(fahr-32) * 5 // 9``` 是一個整數運算式，適用通用的計算規則。因此
fahr-32 必須加上括號。運算子 // 代表整數除法。

字元放在兩個單引號之前，'a' 代表字元 _a_. '%T' 代表特殊字元退格。

# 字元輸入與輸出

核心函式庫讓我們得以讀寫檔案；一個檔案可視為一連串以換行符號分隔的行，每一
行則為一個字元的序列。這樣的觀點是獨立於平台或作業系統的；在某些系統上
(如 Windows)，每一行其實是以兩個字元分隔，分別為 carriage return 與 linefeed。
核心函示庫會對應不同的系統，讓 Eiffel 程式處理的每一行都是以換行字元分隔。

每個 Eiffel 程式會開啟三個標準輸出入檔案，或稱文字串流(text stream)：
standard_input, standard_output, 與 standard_error。
依照預設，standard_input 對應到鍵盤，standard_output 與 standard_error 
對應到螢幕。當使用管線 (pipes) 或輸出入重導時，標準輸出入檔案也指向實體檔案
或暫存緩衝區。程式從 standard_input 讀取並寫出到 standard_output 時並不需要
關心檔案實際指向哪個資源。

類別 ANY 中的 io 查詢回傳一個 STD_FILES 型別的物件讓我們得以操作標準輸出入
檔案。STD_FILES 有許多特徵，以下的程式會用到的重要特徵包含：

```
end_of_file: BOOLEAN
	-- standard_input 在上一次讀取時是否已到達檔案結尾
read_character
	-- 從 standard_input 讀入下一個字元並讓它可透過 last_character 取得。
	-- 若沒有讀取到任何字元，令 end_of_file 為真。
	require
		not end_of_file
	...
last_character: character
	-- 字元，在 read_character 被呼叫時讀取的字元

put_character (c: CHARACTER)
	-- 寫入字元 'c' 到預設輸出的結尾後

注意：EiffelStudio 的 STD_FILES 型別不支援 end_of_file。需使用
io.input.end_of_file。
```

上面只是複製核心函式庫文件 std_files.e 中的一部分。通常 Eiffel 的特徵都帶有
一小段標頭說明，描述特徵的用途與回傳值。

以上所述的四個特徵呈現出一般查詢與命令的分別。read_character 是一個命令，它
嘗試從 standard_input 讀取一個字元並讓 last_character 查詢可取得這個字元，或
是在狀況成立時令 end_of_file 為真，表示 standard_input 中沒有沒有更多的字元。
put_character 寫出一個字元到 default_output，預設為 standard_output。

對 put_character, put_string 等的呼叫可以交錯進行；輸出會依照呼叫的順序呈現。

命令 read_character 有一個前條件(precondiction) (譯：斷言(assertion)的一種)

```
	require not end_of_file
```

意即當上一次讀取輸入串流時遇到了檔案結尾，就不允許呼叫 read_character。

可透過 ace-file 來設定斷言的監控，例如

```
	root
		...
	default
		assertions(all)
  	cluster
		...
	end
```

所有斷言，像是前條件都會在執行期被監控，這對程式的除錯是極佳的輔助。一旦程式
進入成熟並經過完整測試，我們可以改變監控模式為```assertion(no)```而不須更動
任何程式碼。

前條件乃條約式設計(Design by Contract)的一部分，這種設計在 Eiffel 中被廣放的
運用。一個特徵的前條件，建立了客戶端與供給端的一部分條約，並賦予客戶端一項義
務。

- 客戶端義務：只有在前條件成立時才呼叫一個特徵。

條約的另一部分，特過制定後條件(postcondition) 來賦予供給端責任。條約式設計
中，斷言可區分為前條件、後條件、類別不變性(class invariants)、迴圈不變性、
與檢定(checks)。相關資訊在之後的條約式設計章節會提及。

## 檔案複製

僅給予字元輸出入功能時，就能撰寫許多有用的程式。第一個程式是將所有的字元
從輸入複製到輸出。

```
class
	COPY
create
	make
feature
	make
		do
			from
				io.read_character
			until
				io.end_of_file
			loop
				io.put_character (io.last_character )
				io.read_character
			end
		end
end
```

程式碼說明了程式的用意：在迴圈中我們嘗試從輸入串流讀取第一個字元，終止條件
io.end_of_file 檢查是否到達輸入串流的結尾。只要結尾還未到達，剛剛讀取的字元
會以 io.pu_character(io.last_character) 寫入到輸出串流。

這個終止條件保證我們能滿足 read_character 的前條件，也就是不會超過串流結尾後
仍進行讀取。

## 字數計算

稍稍修改檔案複製程式可以讓我們計算輸入串流中的字元數目。

```
class 
	CHAR_COUNT
create
	make
feature
	make
		local
			nc: INTEGER -- 字元數目
		do
			from
				io.read_character
			until
				io.end_of_file
			loop
				nc := nc + 1
				io.read_character
			end

			io.put_string ("number of characters: ")
			io.put_integer (nc)
			io.put_new_line
		end
end
```

相對於將讀取到的字元寫入到輸出串流，我們遞增 nc 這個計數器，最後輸出讀取到的
字元數目。

Eiffel 裡面，大部分的型別有合理的預設值。所有 INTEGER 型態的變數會被初始化為
0 。因此並不需要明確初始化 nc 。

與 C 不同，Eiffel 並沒有遞增運算元(++)，你必須寫成```nc := nc + 1 ```以遞增
 nc 。

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
叢集 cluster
逸出序列 escaped sequence
屬性 attribute
```
