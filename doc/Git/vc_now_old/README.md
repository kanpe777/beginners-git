## 背景的な
昔
 * diffコマンド & patchコマンド

最近
 * Git
 * Mercurial

### 昔の方法でやってみよう
    $ mkdir ~/old
    $ cd ~/old
    $ vim hello.c か emacs -nw hello.c

hello.c (バージョン 1.0)
```
#include <stdio.h>
int main() {
    printf("Hello World\n");
    return 0;
}
```

    $ cp hello.c hello.c.org
    $ diff hello.c.org hello.c

この時点で `diff` コマンド で hello.c.org と hello.c の差分をとろうとしても、全く同じ内容なので、差分なし。

(org = originalの略)

そこで hello.c に変更を加える。
hello.c (バージョン 2.0)
```
#include <stdio.h>
int main() {
    printf("Hello Git\n");
    return 0;
}
```

#### 確認
hello.c.org は `World` , hello.c は `Git` .

diffコマンドで差分を取ると、確認できる。

    $ diff hello.c.org hello.c
    3c3
    <     printf("Hello World\n");
    ---
    >     printf("Hello Git\n");

`diff` コマンドでパッチファイルを作成し、パッチを当てる。

    $ diff -u hello.c.org hello.c > hello.c.patch
    $ patch -u hello.c.org < hello.c.patch
    patching file hello.c.org

パッチが当てられて、差分が適用された。確認は `cat` コマンドでもできる。(diff hello.c.org hello.c でも可)

    $ cat hello.c.org
    #include <stdio.h>
    int main() {
        printf("Hello Git\n");
        return 0;
    }

やっぱりパッチを当てる前に戻したい時、 `-R` オプションをつけるとよい。

    $ patch -R hello.c.org < hello.c.patch
    patching file hello.c.org

これで元に戻る。確認は `cat` コマンド

    $ cat hello.c.org
    #include <stdio.h>
    int main() {
        printf("Hello World\n");
        return 0;
    }

パッチを当てる前に戻ったことを確認。

これらをもっと簡単にできるようにしたツールがバージョン管理システム(VCS)。

その1つがGit。

### 最近の方法でやってみよう
コマンドの意味は後述するので、`おまじない` でok。

とりあえず、便利なところを知ろう。

    $ mkdir ~/now
    $ cd ~/now
    $ vim hello.c か emacs -nw hello.c

先ほど記述した hello.c (バージョン1.0) を記述して、以下のコマンドを実行。

    $ git init
    $ git stage hello.c

これで hello.c を Git というツールで バージョン管理をすることになる

    $ git commit -m 'New file hello.c'

このコマンドで、この時の状態を記録する

    $ vim hello.c か emacs -nw hello.c

そのまま hello.c を バージョン2.0 の状態にして、以下のコマンドを実行

    $ git stage hello.c
    $ git commit -m 'Update hello.c'

これで2つの状態を記録している

    $ git diff HEAD~1
    diff --git a/hello.c b/hello.c
    index 7c29fb1..bed1c31 100644
    --- a/hello.c
    +++ b/hello.c
    @@ -1,6 +1,6 @@
     #include <stdio.h>
     int main() {
    -    printf("Hello World\n");
    +    printf("Hello Git\n");
         return 0;
     }

これは `最新の状態(HEAD)` と `最新状態の1つ前の状態(HEAD~1)` との比較。

では前の状態に戻してみる。

    $ git reset --hard HEAD~1

確認する。

    $ cat hello.c

以上。
