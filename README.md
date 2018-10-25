
## 1. Git

### 1.1 Một số quy tắc của Git 
Có một số quy tắc cần lưu ý:
* Thực hiện công việc trong một branch.
    
    _Vì sao:_
    >Bởi vì cách theo cách này tất các các công việc được hoàn thành trên một branch chuyên dụng riêng thay vì trên cùng một branch chính. Nó cho phép bạn gửi nhiều pull request mà không bị nhầm lẫn. Bạn có thể lặp lại mà không làm hỏng branch chính với khản năng không ổn định, code chưa hoàn chỉnh. [Xem thêm...](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow)
* Chia nhánh từ nhánh `develop`
    
    _Vì sao:_
    >Theo cách này, Bạn có thể bảo đảm code master trong sẽ luôn luôn được xây dựng mà không có vấn đề, và có thể chủ yếu được sử dụng trực tiếp cho các nhà phát hành  and can be mostly used directly for releases (điều này có thể là quá mức cần thiết với một số dự án).

* Không bao giờ push vào nhánh `develop` hoặc `master` . Hãy pull.
    
    _Vì sao:_
    > Điều đó thông báo với các thành viên trong nhóm rằng họ đẫ hoàn thành một tính năng. Nó cho phép dễ dàng so sánh code và đăng lên diễn đàn để thảo luận về tính năng mới.

* Cập nhật nhánh local `develop` của bạn và thực hiện rebase trươc khi push tính năng của bạn và tạo một pull request.

    _Vì sao:_
    > Rebasing sẽ hợp nhất trong các nhánh được yêu cầu(`master` hoặc `develop`) và áp dụng các commit mà bạn đã tạo ở local lên top của lịch sủ mà không cần tạo một merge commit (Giả sử không có xung đột). Kết quả là có một lịch sủ đẹp và rõ ràng. [Xem thêm ...](https://www.atlassian.com/git/tutorials/merging-vs-rebasing)
conflicts
* Giải quyết các khản năng xung đột trong khi rebasing và trược khi thực hiện một Pull Request.
* Xóa cách nhánh cục bộ và remote các nhánh sau khi hợp nhất.
    
    _Vì sao:_
    > Nó sẽ chộn lẫn danh sách nhánh của bạn với các nhánh chết. Nó sẽ đảm bảo bạn hợp nhất các nhánh vào (`master` hoặc `develop`) một lần duy nhất. Các nhánh tính năng chỉ nên tồn tại trong khi công việc vẫn đang được tiến hành.

* Trước khi thực hiện Pull Request, chắc chắn ràng các nhánh tính năng được xây dựng thành công và vượt qua tất cả các kiểm tra (bao gồm cả kiểm tra code style).
    
    _Vì sao:_
    > Bạn săp thêm code của bạn vào một nhánh ổn định. Nếu tính năng của bạn  kiểm tra fail, có khả năng cao là việc xây dựng nhánh đích của bạn cũng fail. Ngoài ra, bạn cần áp dụng kiển tra code style trước khi thực hiện một Pull Request. Nó hỗ trợ khả năng đọc và làm giảm khả năng phải sửa lỗi định dạng được trộn lẫn với thay đổi thực tế.

* Sử dụng file `.gitignore` .
    
    _Vì sao:_
    > Nó có danh sách các file hệ thống không được gửi cùng với code của bạn vào một kho chứa từ xa. Ngoài ra, nó không bao gồm các file và thư mục cài đặt trong hầu hết các editor, cũng như hầu hết các thư mục phụ thuộc phổ biến nhất.

* Bảo vệ nhánh `develop` và `master` của bạn.
  
    _Vì sao:_
    > Nó bảo vệ các nhánh của bạn khỏi các tác nhân bất ngờ và không thể đảo ngược. Xem thêm... [Github](https://help.github.com/articles/about-protected-branches/), [Bitbucket](https://confluence.atlassian.com/bitbucketserver/using-branch-permissions-776639807.html) và [GitLab](https://docs.gitlab.com/ee/user/project/protected_branches.html)


### 1.2 Quy trình làm việc của Git 
Vì hầu hết các lý do trên, chúng ta sử dụng  [Quy trình làm việc của Feature-branch](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) với [Tương tác Rebasing](https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing) và một số thành phần của [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows#gitflow-workflow) (đặt tên và có một nhánh devolop). Các bước chính như sau:

* Đối với một dự án mới, Khởi tạo một kho git trong danh mục dự án. __Đối với các tính năng/thay đổi tiếp theo bược này được bỏ qua__.
   ```sh
   cd <project directory>
   git init
   ```

* Kiểm tra một nhánh feature/bug-fix mới.
    ```sh
    git checkout -b <branchname>
    ```
* Thực hiện thay đổi.
    ```sh
    git add
    git commit -a
    ```
    _Vì:_
    > `git commit -a` bắt đầu quá trình sửa cho phép bạn tách đối tượng ra khỏi body. Độ thêm về nó trong *mục 1.3*.

* Đồng bộ hóa các thay đổi bạn đã quên.
    ```sh
    git checkout develop
    git pull
    ```
    
    _Vì:_
    > Điều này sẽ cho phép bạn dễ dàng khắc phục các xung đột trên máy của bạn trong khi rebasing (sau này) hơn là tạo một pull request có chứa xung đột.
    
* Cập nhật nhánh tính năng với những thay đổi mới nhất từ devolop với tương tác rebasr 
    ```sh
    git checkout <branchname>
    git rebase -i --autosquash develop
    ```
    
    _Vì:_
    > Bạn có thể sử dụng --autosquash để đè tất các các commit của bạn lên một commit. Không ai muốn nhiều commit trong một nhánh tính năng duy nhất. [Xem thêm...](https://robots.thoughtbot.com/autosquashing-git-commits)
    
* Nếu bạn không có xung đột, bỏ qua bước này. Nếu bạn có xung đột, [giải quyết chúng](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)  và tiếp tục rebase.
    ```sh
    git add <file1> <file2> ...
    git rebase --continue
    ```
* Push nhánh của bạn. Rebase sẽ thay đổi lịch sử, vì vậy bạn phải sử dụng  `-f` để ép thay đổi và các nhánh từ xa. Nếu ai đó đang làm việc trên nhánh của bạn , sử dụng ít phá hủy hơn `--force-with-lease`.
    ```sh
    git push -f
    ```
    
    _Vì:_
    > Khi bạn thực hiện một rebase,  bạn sẽ thay đổi lịch sử trên nhách chức năng của bạn.Kết quả là, Git sẽ từ chối `git push`. Thay thế, bạn sẽ cần sử dụng -f hoặc --force . [Xem thêm...](https://developer.atlassian.com/blog/2015/04/force-with-lease/)
    
    
* Thực hiện một Pull Request.
* Pull request sẽ được chấp nhận, hợp nhất và thoát bởi một reviewer.
* Xóa nhánh local của bạn nếu bạn đã hoàn thành.

  ```sh
  git branch -d <branchname>
  ```
  để xóa các nhánh không còn remote
  ```sh
  git fetch -p && for branch in `git branch -vv --no-color | grep ': gone]' | awk '{print $1}'`; do git branch -D $branch; done
  ```


### 1.3 Viết một commit tốt

Có một hướng dẫn tốt cho việc tạo và gắn kết các commit làm cho việc làm việc với Git dễ dàng hơn rất nhiều. Dưới đây là một số quy tắc  ([source](https://chris.beams.io/posts/git-commit/#seven-rules)):

 * Tách một đối tượng từ phần chính với một dòng mới giữa 2 dòng.

    _Vì:_
    > Git đủ thông minh để phân biệt dòng đầu tiên trong commit của bạn dưới dạng tóm tắt. Trong thực tế, nếu bạn cố gắng thử git shortlog, Thay vì đăng nhập git, bạn sẽ nhìn thấy một danh sách dài các commit messages, bao gồm id của commit và bản tóm tắt.

 * Giới hạn dòng chủ đề ở 50 ký tự và phần chính là 72 kí tự.

    _Vì_
    > Các Commit cần được tinh chỉnh và tập trung hết mức co thể, đó không phải nơi để dài dòng. [Xem thêm...](https://medium.com/@preslavrachev/what-s-with-the-50-72-rule-8a906f61f09c)

 * Viết hoa dòng chủ đề.
 * Không được kết thúc dong chủ để với một khoản trống.
 * Sử dụng [Tình trạng bắt buộc](https://en.wikipedia.org/wiki/Imperative_mood) trong các dòng chủ đề.

    _Vì:_
    > Thay vì viết tin nhắn cho biết những gì người commit đã làm. Tốt hơn là xem các thông báo này như hướng dẫn cho những gì sẽ thực hiện sau khi commit được áp dụng trên repository. [xem thêm...](https://news.ycombinator.com/item?id=2079612)


 * Sử dụng phần thân để giải thích **Cái gì** và **Tại sao** trái ngược với **Làm sao**.


