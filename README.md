TodoMVC の repo から VanillaJS のコードを取り出す
==================================================

目的
----

TodoMVC の examples/vanillajs/ 以下だけを、その履歴を保持しつつ別な repo に取り出す

課題
----

 * git filter-branch は index を上書きしてしまうので一部を取り出すことは達成できるが元の repo を破壊するので持続できない
 * todomvc は全部の examples を内包しており巨大なので直接書き換えてしまうと次回以降の作業負荷が大きい

できれば todomvc の repo は書き換えずに継続的に pull できる状態を維持したい。

解決方法
--------

 * clone / pull 用の todomvc repo から作業用の repo を cp
 * vanillajs 用に cp した repo 上で git filter-branch で vanillajs だけの repo を生成
 * remote に push -f

