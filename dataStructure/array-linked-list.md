# Array & Linked List

## **Array**

Array 자료구조는 논리적 , 물리적 저장 순서가 일치하며 인덱스로 원하는 원소에 접근이 가능합니다.

인덱스 값만 알고 있다면 0(1)의 시간 복잡도를 가지게 됩니다.

하지만 원소 삭제,삽입의 과정에 있어서는 배열의 특징이 깨지기 때문에 O(n)의 복잡도를 가지는 작업이 된다.

- 삭제의 경우에 삭제된 원소 이후에 배열의 원소들은 모두 인덱스가 -1씩 이동하는 작업을 해야 한다.
- 삽입의 경우에 삽입된 원소 이 후 배열의 원소들은 인덱스가 +1씩 이동하는 작업을 해야 한다.#

## **Array List**

Array List는 Array와 다르게 각각 원소(Node)들은 자기 다음에 원소가 누구인지만 알고 있다.

따라서 삭제, 삽입 하고 자는 요소들의 위치만 알고 있다면 O(1)의 허리접 도를 가지게 된다.하지만 해당하는 Node의 위치를 찾는 과정에서 Array와 다르게 index로 바로 접근할 수 없다. 왜냐하면 각각의 Node는 자신의 다음 요소밖에 알고 있지 않기 때문이다. 이런 이유로 Search에서는 O(n)의 복잡도를 가지게 된다.

- Linked List 추가 삭제 예제

```jsx
class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
  }

  add(value) {
    const node = new Node(value);

    if (!this.head) {
      this.head = node;
      this.tail = node;
    } else {
      this.tail.next = node;
      this.tail = node;
    }
  }

  find(value) {
    let node = this.head;

    while (node !== null) {
      if (node.value === value) {
        return node.value;
      } else {
        node = node.next;
      }
    }

    return node;
  }

  showLinkedList() {
    console.log(this.head);
  }
}
```
