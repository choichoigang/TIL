# DOCTYPE

- "문서 형식 선언"(Document Type Declaration) 또는 DOCTYPE이란 어떤 SGML이나 XML 기반 문서 내에 그 문서가 특정 문서 형식 정의(DTD)를 따름을 지정합니다.

## Quirks Mode

- Quirks mode는 오래된 웹 브라우저들을 위해 디자인된 웹 페이지의 하위 호환성을 유지하기 위해 W3C나 IETF의 표준을 엄격히 준수하는 Standards Mode를 대신하여 사용되는 웹 브라우저의 기술을 나타냅니다.

## Older HTML Documents

- <HTML 5>현재 가장 최신의 HTML 선언 방식은 아주 간단합니다.

```yaml
<!DOCTYPE html>
```

- 이전 문서 (HTML 4 또는 XHTML)에서는 선언이 DTD (Document Type Definition)를 참조해야 하므로 선언이 더 복잡했습니다.

```yaml
// HTML 4.01
**<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
// XHTML 1.1
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">**
```
