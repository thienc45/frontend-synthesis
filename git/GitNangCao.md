# Git nâng cao

## 1 Thao tác với branch

Khi làm việc trong một dự án có nhiều người tham gia, chúng ta thường sẽ tạo nhiều nhánh để dễ dàng quản lý code hơn. Ví dụ: Người A code tính năng login ở nhánh `feature/Login` và người B code tính năng register ở nhánh `feature/Register`, 2 người không ảnh hưởng đến nhau khi code. Sau đó, hai người sẽ tiến hành gộp code của họ lại vào `master` khi họ code xong.

### 1.1 Tạo branch mới

Câu lệnh này sẽ tạo một nhánh mới dựa trên nhánh hiện tại

```bash
git branch TenNhanhMoi
```

2 câu lệnh dưới sẽ tạo một nhánh mới dựa trên nhánh hiện tại và chuyển sang nhánh mới luôn.

```bash
git checkout -b TenNhanhMoi
```

```bash
git switch -c TenNhanhMoi
```

`git switch` mới ra ở phiên bản Git 2.23. Các bạn dùng cái nào cũng được

### 1.2 List tất cả branch

Câu lệnh này sẽ cho bạn biết local repo của bạn hiện tại đang có bao nhiêu nhánh. Những nhánh mà câu lệnh `git branch` show ra là những nhánh mà bạn đã từng làm việc trên local. Có những nhánh đã có ở local mà bạn chưa làm việc (những nhánh này thường là do bạn pull hoặc fetch code về sẽ có) thì nó sẽ không hiện ở đây.

```bash
git branch
```

hoặc `git branch --list` đều như nhau

Nếu muốn thấy các remote branch

```bash
git branch -r
```

Nếu muốn thấy tất cả local và remote branch

```bash
git branch -a
```

### 1.3. Đổi tên branch

Nếu bạn tạo tên nhánh bị sai, bạn cũng có thể sửa lại tên nhánh hiện tại bằng cách

```bash
git branch -m TenMoi
```

Cách trên chỉ áp dụng khi nhánh hiện tại của bạn là nhánh sai và bạn muốn đổi tên nhánh hiện tại. Trong trường hợp bạn muốn đổi tên nhánh nào đó không nhất thiết là nhánh hiện tại thì bạn phải ghi rõ tên nhánh cũ và nhánh mới như dưới đây

```bash
git branch -m TenNhanhCu TenNhanhMoi
```

Các bạn đổi tên branch thì chỉ đổi được ở dưới local repo, nếu branch đó đã xuất hiện ở trên remote repo thì khi bạn push nó sẽ tạo một branch mới trên remote repo

### 1.4 Chuyển branch

Nếu bạn đơn giản chỉ muốn chuyển sang một nhánh trên local repo, 2 câu lệnh dưới đây đều được

```bash
git switch TenNhanh
```

```bash
git checkout TenNhanh
```

> **Lưu ý**: Nhánh phải có trên local repo thì mới chuyển được nhé, nếu nhánh đó có trên remote repo mà local chưa có thì bạn phải tiến hành cập nhật repo của bạn lại bằng câu lệnh `git fetch`.

### 1.5 Push một branch

Nếu local branch của bạn không tồn tại trên remote, chạy câu lệnh sau

```bash
git push -u origin localBranch
```

Hoặc câu lệnh này, nó sẽ giúp các bạn không cần gõ chính xác tên local branch

```bash
git push -u origin HEAD
```

> Head là tham chiếu đến đầu danh sách branch hiện tại

### 1.6 Xóa branch

Xóa branch ở local

```bash
git branch -D localBranchName
```

Xóa branch ở remote

```bash
git push origin --delete remoteBranchName
```

Hoặc bạn có thể dùng cú pháp rút gọn

```bash
git push origin :remoteBranchName
```

Nếu bạn gặp lỗi dưới đây thì có thể ai đó đã xóa branch

```bash
error: unable to delete 'branchName': remote ref does not exist
error: failed to push some refs to 'git@repository_name'
```

Khi ai đó đã xóa một branch trên remote nhưng khi bạn gõ `git branch -r` vẫn show ra origin branch đó thì bạn cần thực hiện đồng bộ hóa bằng câu lệnh dưới đây.

```bash
git fetch -p
```

`-p` nghĩa là **"prune"**. Sau khi fetch, những branch không còn trên remote cũng sẽ xóa khỏi local repo của bạn.

### 1.7 Push code mà không cần origin

Nếu bạn để ý thì khi ở nhánh `master`, bạn push code lên orgin thì bạn chỉ cần `git push` là xong, không cần `git push origin master`. Nhưng nếu bạn ở nhánh khác ví dụ nhánh `feature` thì khi bạn push code lên `origin feature` thì phải `git push orgin feature` khá dài dòng và phải nhớ tên nhánh mệt mỏi.

Cách cải thiện khá đơn giản, bạn chỉ cần thêm option `-u` câu lệnh push là được, chỉ cần làm 1 lần, lần sau không cần làm với nhánh đó nữa.

```bash
git push -u origin feature
```

`-u` là short-hand của `--set-upstream`. Nó cho git biết rằng hãy tự kết nối nhánh `local feature` với `origin feature`

Làm 1 lần thôi, lần sau chỉ cần ở nhánh `feature` và nhấn `git push` là code tự lên.

> Tip: Nếu muốn xem những local branch đã kết nối với remote branch thì chỉ cần gõ `cat .git/config`

### git merge

Gộp các commit lại của 2 nhánh với nhau dựa trên thời gian các commit, sau khi gộp thì sẽ tạo ra một commit gộp sau cùng.

```bash
git merge branchTarget
```

Chúng ta có 3 nhánh:

- main
- Alpha: checkout từ nhánh main và thêm 2 commit D,E
- Beta: checkout từ nhánh main và thêm 2 commit là F,G

Thứ tự các commit theo chiều ngang được sắp xếp theo thời gian

```shell
   (main)
A--B-C---D--E (Alpha)
      \
       -F-----G (Beta)
```

Nếu ở nhánh Beta chúng ta muốn có những commit của nhánh Alpha (hay còn gọi là có code) thì ta sẽ tiến hành merge Alpha vào Beta nên ta thực hiện lệnh

```bash
git checkout Beta
git merge Alpha
```

Sơ đồ commit sẽ như thế này, các commit của Alpha sẽ xuất hiện trong Beta và được sắp xếp theo thứ tự thời gian. Ta sẽ thấy sự xuất hiện của một commit merge nữa phía sau cùng, commit merge này chứa tất cả những sự thay đổi của nhánh Beta sau khi merge nhánh Alpha vào.

```shell
   (main)
A--B-C---D--E (Alpha)
      \       \
       -F----G-M (Beta)
```

Bây giờ nhánh Beta đã có đầy đủ code của nhánh Alpha.

Nếu trong quá trình merge có bị conflict thì thao tác theo thứ tự

1. Tìm file conflict trong tab source và fix nó.
2. Sau khi fix hết các file bị conflict rồi thì dùng `git add .` hoặc add từng file
3. Tiến hành thêm commit cho những file vừa fix conflict bằng câu lệnh `git commit --no-edit`, nếu muốn edit commit thì `git merge --continue`, còn nếu muốn tự viết commit thì cứ `git commit -m 'thông điệp'` như thường
4. push hết lên với câu lệnh `git push`

Nếu merge xong rồi mới nhận ra mình không cần merge nữa thì có thể dùng `git reset --hard <commit-của bạn-trước-khi-merge>`. Cách này áp dụng cho cả đã push code hay chưa push đều được.

> Tips: `git pull` là sự kết hợp giữa `git fetch` và `git merge`. Trong thực tế thì mình dùng `git pull` chứ ít khi dùng `git merge` vì mình luôn muốn fetch data mới nhất từ origin về rồi mới merge.
>
> Bạn cũng có thể tạo merge request (một số chỗ gọi là pull request) bằng git server như Github thay vì dùng câu lệnh. Những merge request này được tạo ra để phục vụ việc team review và approve code.

### git rebase

Lấy tất cả các commit của `branchTarget` làm lại base (gọi là re-base) cho mình, các commit của mình mà khác so với `branchTarget` sẽ bị thay đổi hash và thêm vào sau cùng.

```bash
git rebase branchTarget
```

```txt
   (main)
A-B-C-----D (feature)
     \
      --E----F (feature/Login)
```

```bash
git checkout feature/Login
git rebase feature
```

```txt
   (main)(feature)
A-B-C-----D--E'----F' (feature/Login)
```

Nếu những commit của branch `feature/Login` và `feature` đã xuất hiện trên remote thì có thể sau khi rebase `feature` vào `feature/Login` thì bạn sẽ gặp một hiện tượng đó là git sẽ báo bạn kiểu như là **"Current branch feature/Login is 2 commit behind, 3 commit ahead origin/feature/Login"**.

Giải thích:

- `feature/Login` ở local chỉ giống với `origin/feature/Login` đoạn `A-B-C` thôi =>
  `feature/Login` đứng sau 2 commit của `origin/feature/Login` là `E-F`. Ngoài ra `feature/Login` có 3 commit trước so với `origin/feature/Login` là `D--E'----F'`

- Git sẽ yêu cầu bạn pull code từ origin `feature/Login` về trước khi push lên. Mọi thao tác làm thay đổi lịch sử Git trên origin như xóa commit, đổi mã hash commit thì Git đều không cho push mà yêu cầu pull về trước. Nếu pull code về thì ta lại có commit `E-F` khá thừa thải vì đã có `E'-F'` rồi, để tránh hiện tượng này thì bạn có thể push force `git push -f` luôn.

Git rebase sẽ làm Git Graph của bạn trông đẹp, dễ nhìn, đỡ phải phân tách nhánh rồi gộp như bên rebase. Nhưng cái gì cũng có cái giá của nó. Bạn phải sử dụng `git push -f` sau cùng, và câu lệnh này **rất nguy hiểm** nếu bạn đang thực hiện trên một branch nhiều người dùng.

Ở trên mình đang thực hiện trên branch `feature/Login`, mình rebase `feature` vào. Nếu mình làm người lại là rebase `feature/Login` vào `feature` thì rất nguy hiểm.

### Hãy cẩn thận với git rebase

**Không nên rebase những nhánh khác vào nhánh tổng (nhánh có nhiều người làm gốc để tạo nhánh con, nhánh được chia sẻ bởi nhiều người).** Ví dụ bạn đang ở nhánh `master`, bạn muốn có code của nhánh `feature/Login` và bạn thực hiện

```bash
git checkout master
git rebase feature/Login
```

Lúc này thì base lịch sử commit của nhánh `master` sẽ bị **thay thế**. Nó sẽ lấy base là nhánh `feature/Login` và đặt những commit mới của `master` ra sau cùng, điều này không tốt chút nào với một nhánh tổng. Việc **thay thế** base lịch sử commit của `master` sẽ gây khó khăn trong việc revert tính năng Login, bạn phải `git revert` tất cả commit của tính năng Login từng cái một.

Khi rebase thì phải push force, mà push force rất nguy hiểm, nhất là trên nhánh tổng. Vì nó sẽ overide code của người khác đã commit trên nhánh tổng đó.

Nhưng nó sẽ là an toàn nếu bạn đang ở nhánh `feature/Login` (đây là nhánh chỉ có mỗi 1 mình bạn đảm nhận code) và bạn muốn có code mới nhất của nhánh `master`, bạn thực hiện

```bash
git checkout feature/Login
git rebase master
```

Ưu điểm rebase:

- Tạo một đường thằng commit sạch sẽ, dễ nhìn
- Nếu làm trên nhánh cá nhân thì sẽ phù hợp

Khuyết điểm

- Nếu rebase trên branch chung thì sẽ rất nguy hiểm vì có thể làm mất code người khác
- Dùng push force có thể làm mất code nếu không để ý
- Muốn undo rebase thì không phải chuyện đơn giản. Nếu bạn rebase trên máy bạn thì bạn có thể undo bằng cách sử dụng `git reflog` và `git reset` nhưng nếu bạn clear local repo hoặc bạn phải thực hiện undo rebase trên máy khác thì bạn phải dùng `git revert` để hoàn tác từng commit.

Cách an toàn nhất trong mọi trường hợp là cứ dùng `git merge` đi, mặc dù merge nó sẽ tạo ra thêm 1 commit merge và lịch sử commit của branch đó sẽ bị thêm khá nhiều commit xen kẻ của người khác làm cho bạn rối nhưng ít ra nó sẽ không làm cho bạn bị mất code.

### git fetch

Phổ biến nhất là chỉ cần gõ `git fetch`, nó sẽ load về local tất cả những branch, commit hiện có.

### git remote

Lệnh `git remote` cho phép tạo, xem, xóa kết nối với các repo.

Để liệt kê các liên kết

```bash
git remote
```

> Khi lấy một remote reop bằng câu lệnh `git clone`, mặc định repo tải về có liên kết với tên `origin` chứa địa chỉ tham chiếu đến remote repo nó tải về

Hiển thị thông tin chi tiết hơn, có thêm đường dẫn đến remote Repo

```bash
git remote -v
```

Tạo một liên kết

```bash
git remote add TenRemote url
```

Xóa một địa chỉ remote

```bash
git remote rm TenRemote
```

Đổi tên địa chỉ remote

```bash
git remote rename TenCu TenMoi
```

Xem thông tin về Remote

```bash
git remote show origin
```
