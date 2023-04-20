# Common Tools

## Linux 环境配置相关

1.conda配置文件路径

```
/home/username/.condarc
```

2.pip配置文件路径

```
/home/username/.config/pip
```

3.创建新用户

```
useradd -d /name -s /bin/bash -m name
passwd name
```

## Git

1. lfs使用：https://git-lfs.github.com/

2. 撤销 git add 操作

   ```
   git status # 先看一下add 中的文件 
   git reset HEAD # 如果后面什么都不跟的话 是上一次add 里面的全部撤销了 
   git reset HEAD XXX.py # 是对某个py文件进行撤销了
   git reset HEAD file  # 对file文件夹进行撤销
   ```

3. git 切换分支的互相影响

   参考：https://blog.csdn.net/weixin_34114823/article/details/92254904

   * git的规则：在本地修改的文件，在哪个分支上提交就算那个分支的修改。即如果修改本地文件后，不提交就切换分支，则会将修改带到切换后的分支中
   * 可取方法
     * 跳转分支之前使用 git status查看是不是有没有add和commit的工作。
       * 如果有，且可提交，就直接提交掉。
       *  如果确实有尚未add和commit的工作，但是并未完成不方便进行提交，可以利用git stash进行现场保留，然后跳转。
       *  如果前两步都没有做，带着未commit的工作跳转到了另一分支下，不做任何修改时可直接跳回原分支，进行1或2步操作。
       * 直接再跳回去就好，然后进行步骤1或2。

4. git stash

   参考：https://blog.csdn.net/stone_yw/article/details/80795669

   ```
   git stash
   git stash save "test1"
   git stash list
   git stash pop
   git stats apply
   git stash drop 名称 # 从堆栈中移除某个指定的stash
   git stash clear # 清除堆栈中的所有内容
   git stash show # 查看堆栈中最新保存的stash和当前目录的差异
   git stash show stash@{1} #查看指定的stash和当前目录差异。
   git stash show -p  # 查看详细的不同
   git stash branch # 从最新的stash创建分支
   ```

   git stash pop

   * 将当前stash中的内容弹出，并应用到**当前分支**对应的工作目录上
   * 注：该命令将堆栈中最近保存的内容删除（栈是先进后出）
   * 代码冲突处理：<<<<<<< Updated upstream到=======之间的代码是拉取的别人的代码，=======到>>>>>>> Stashed changes是你自己本次修改的代码

   git stash apply

   * 将堆栈中的内容应用到当前目录，**不同于git stash pop，该命令不会将内容从堆栈中删除**，也就说该命令能够将堆栈的内容多次应用到工作目录中，适应于多个分支的情况
   * 堆栈中的内容并没有删除
   * 可以使用git stash apply + stash名字（如stash@{1}）指定恢复哪个stash到当前的工作目

   git stash branch

   * 应用场景：当储藏了部分工作，暂时不去理会，继续在当前分支进行开发，后续想将stash中的内容恢复到当前工作目录时，如果是针对同一个文件的修改（即便不是同行数据），那么可能会发生冲突，恢复失败，这里通过创建新的分支来解决。可以用于解决stash中的内容和当前目录的内容发生冲突的情景。
   * 发生冲突时，需手动解决冲突。

## 使用举例

1. stash

   ```
   git stash
   git commit
   git stash pop
   git stash apply
   git stash clear
   ```

2. 放弃本地修改，直接覆盖

   ```
   git reset --hard
   git pull
   ```

3. 合并两个分支内容

   1. 将开发分支代码合入master分支中

   ```
   git checkout dev           #切换到dev开发分支
   git pull
   git checkout master
   git merge dev              #合并dev分支到master上
   git push origin master     #将代码推到master上
   ```

   1. 将master分支的代码同步更新到开发分支中

        merge方法：保证主干提交线干净(可以安全回溯)

   ```
   git checkout master
   git pull
   git checkout dev
   git merge master
   git pull origin dev
   ```
## FFmpeg

1. 查看版本

   ```
   ffmpeg -version
   ```

2. 截取动图

   ```
   ffmpeg -i video.mp4 -s 1920*1080 -r 25 res.gif
   ```

3. Convert MP4 VersionISO Media v1 转换为 v2

   ```
   ffmpeg -i out.mp4 -c copy -map 0 -brand mp42 out2.mp4
   ```

3. rescale 视频

   ```
   ffmpeg -i orig.mp4 -vf scale=640:480:flags=bicubic -c:v libx264  -crf 18 output.mp4
   ```

crf 是控制质量的，常用18，crf越小编码质量越高，18大致是视觉无损，要求再高的话就降到10，再往下降的话输出视频的体积就会显著增大了
