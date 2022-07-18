---
title: "Java: JVM、JRE 和 JDK 的差別"
date: 2022-07-18T08:19:36.492Z
author: Boison
slug: java-jvm-jre-jdk
tags:
  - Java
  - Runtime
draft: false
---
> **Java 同時採用直譯器 & 編譯器的混和模式來翻譯程式**

1. **編譯器**

   * 傳統編譯器會把原始程式通過語法跟語譯的分析把他們轉換為 IR 也就是所謂中間碼
   * IR 格式已經與組合語言相近了，之後的步驟都是在最佳化及產生目的碼
2. **直譯器**

   * 直譯器則會讀入原始程式後一行一行的解釋並執行，導致他的速度相較之下較慢

Java 則不同其他程式語言，為了達到他的跨平台性，他採用了上述兩種混和的模式: 取出編譯器中與機器無關的處理程序，後面交由 JVM 直譯器來執行。

※ Java 直譯器是 JVM 的一部分
- - -
## 1. JVM 是 Java 的編譯器

>  **JVM 是 Java 的編譯器，支援與操作系統無關的部分實現 Java 跨平台運行的特性**

JVM（Java Virtual Machine）是 JRE 的一部分，JVM 就好像是一台虛構的電腦，運行在實體電腦裡，是通過在實際的計算機上仿真模擬各種計算機功能來實現的。JVM 有自己完善的硬件架構，如處理器、堆棧、寄存器等，還具有相應的指令系統。

一般的高階語言如果要在不同的平臺上執行，至少需要編譯成不同的目的碼。而引入 JVM（Java Virtual Machine）後，Java 在不同平臺上執行時不需要重新編譯。Java 語言使用 JVM 遮蔽了與具體平臺相關的資訊，使得 Java 語言編譯程式只需生成在 JVM 上執行的目的碼（位元組碼），就可以在多種平臺上不加修改地執行。

JVM 在執行位元組碼時，把位元組碼解釋成具體平臺上的機器指令執行。這就是 Java 能夠一次編譯，到處執行的原因。

- - -

## 2. JRE 是 Java 的執行環境

JRE（Java Runtime Environment），JRE 稱為 Java 執行環境 Runtime，是一個由 Java API 類庫中的 Java SE API 子集和 JVM 組成。

JRE 是個執行環境，JRE 只能執行不能編譯！也就是說如果你只是下載 JRE 你不能編寫 Java。

- - -

## 3. JDK 是 Java 的開發環境

JDK 是整個 Java 的核心，包括了 Java 執行環境 JVM（Java Runtime Envirnment），一堆Java 工具（javac/java/jdb 等）和 Java 基礎的類庫（即 Java API ）。

JDK（Java Development Kit）是個開發環境，編寫 Java 程序的時候需要 JDK，而運行 Java 程序的時候就需要 JRE，而 JDK 裏面已經包含了 JRE，因此只要安裝了 JDK，就可以編輯 Java 程序，也可以正常運行 Java 程序。

- - -

> **參考資料**
>
> 1. [JVM 以及 JDK 的簡介](https://ithelp.ithome.com.tw/articles/10220205)
> 2. [什麽是JDK？什麽是JRE？JDK與JRE的區別和用途](https://www.796t.com/content/1538401203.html)
> 3. [何謂 JVM、JRE、SDK、JDK](https://www.laird.tw/2015/03/java-jvmjresdkjdk.html)
> 4. [《java為何這麼簡單》基礎篇-車的發動機JDK](https://www.796t.com/content/1541934811.html)
> 5. [為什麼需要 JVM？](https://openhome.cc/Gossip/JavaEssence/WhyJVM.html)