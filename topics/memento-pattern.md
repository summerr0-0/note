# memento-pattern
* 객체의 상태를 이전 상태로 복원하는 패턴
* 객체가 자신의 상태를 어떤 시점에 저장하고 필요할 때 그 시점의 상태로 복원 가능

### 구성
Originator(발생자)
* 상태를 저장해야될 객체
* `Originator`는 메멘토 객체를 생성해서 현재 상태를 저장하고 메멘토를 이용해서 이전으로 복원 가능 
Memento(메멘토)
* `Originator` 객체의 상태를 저장하는 객체
* `Originator`의 내부 상태를 그대로 가지고 있지만 그 내부 상태에는 직접 접근할 수 없도록 캡슐화
Caretaker(관리자)
* `Memento`를 보관하는 객체
* `Memento`를 저장하거나 복원하는 역할이지만 메멘토의 내용을 조작하거나 직접 접근하면 안된다

### 대표적 예시
* 텍스트 에디터의 Undo
* 체크포인트
* 버전관리 시스템

### 게시글 이전 버전으로 되돌리기
```Java

// Originator
//실제 게시글
//memento를 통해 이전 상태로 복원한다
class Post {
    private String content;
    private LocalDateTime editTime;

    public void setContent(String content) {
        this.content = content;
        this.editTime = LocalDateTime.now();
    }

//이전 상태 저장
    public PostMemento save() {
        return new PostMemento(this.content, this.editTime);
    }

//memento를 이용해서 이전 값 가져온다
    public void restore(PostMemento memento) {
        this.content = memento.getContent();
        this.editTime = memento.getEditTime();
    }
}

// Memento
// 댓글의 상태를 내부적으로 저장
// 외부에서 이 값을 수정할 수 없음
class PostMemento {
    private final String content;
    private final LocalDateTime editTime;

    public PostMemento(String content, LocalDateTime editTime) {
        this.content = content;
        this.editTime = editTime;
    }

    public String getContent() {
        return content;
    }

    public LocalDateTime getEditTime() {
        return editTime;
    }
}

// Caretaker
//사용자가 글을 수정할 때마다 Memento 객체 저장
class EditHistory {
    private final List<PostMemento> history = new ArrayList<>();

    //글을 수정할 때마다 Originator에서 생성된 Memento 객체 저장
    //사용자가 특정 버전으로 롤백을 요청할 때 해당 Memento 객체를 Originator에 전달해서 상태를 복원한다
    public void saveMemento(PostMemento memento) {
        history.add(memento);
    }

    public PostMemento getMemento(int index) {
        return history.get(index);
    }
}

//사용
public class Main {
    public static void main(String[] args) {
        // 게시물 생성 및 초기 상태 설정
        Post post = new Post();
        post.setContent("처음 작성된 게시물 내용");
        
        // EditHistory 객체 생성
        EditHistory history = new EditHistory();
        
        // 현재 상태를 저장
        history.saveMemento(post.save());

        // 게시물 수정 1차
        post.setContent("첫 번째로 수정된 게시물 내용");
        // 수정된 상태를 저장
        history.saveMemento(post.save());

        // 게시물 수정 2차
        post.setContent("두 번째로 수정된 게시물 내용");
        // 수정된 상태를 저장
        history.saveMemento(post.save());

        // 이전 상태로 복원해보기
        // 예를 들어, 첫 번째 수정 상태로 되돌아가기
        post.restore(history.getMemento(1)); // 인덱스는 0부터 시작하므로, 첫 번째 수정 상태는 인덱스 1에 해당

        System.out.println("복원된 게시물 내용: " + post.getContent());
        // 출력: 복원된 게시물 내용: 첫 번째로 수정된 게시물 내용
    }
}
```