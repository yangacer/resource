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
類別名稱通常都是以大寫表示，而功能 (feature) 通常是以小寫表示。

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
；所有類別、功能或變數不能使用這些保留字。

現在，我們來看看上面程式的結構

```
class
	HELLO	-- 類別名稱
create
	make	-- 建構程序
feature
	...	-- 類別功能
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

