#  NoSQL

- 초기에는 표준 SQL 인터페이스를 채용하지 않아 NoSQL이라 불렸으나 현재는 SQL 계열 쿼리 언어를 사용할 수 있다는 사실을 강조하면서 "Not only SQL"이라고도 불린다. ([출처]( https://ko.wikipedia.org/wiki/NoSQL ))

## Aggregation

- [참고 링크]( https://docs.mongodb.com/manual/aggregation/ )

### Aggregation Pipeline

- Stage별로 진행된다. (SQL처럼 한 번에 다 써야 하는 것은 아님)
- 대부분의 stage는 **filtering**과 **document transformation**이다.
- 나머지는 그루핑과 정렬 정도
- Stage에서 operator를 사용할 수 있다. (문자열 병합이나 평균 계산 등)

### Map-Reduce

- Map-reduce operation도 제공
- Map 단계에서는 각 document들을 process하고 각각에 대한 한 개 이상의 object를 **emit**한다.
- Reduce 단계에서는 map 단계의 output을 combine한다.

### Match

```nosql
$match: {
	필드명: {조건}
}
```

- 조건에 `$gte` 등 사용 가능

## Update

```
update(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>
	}
)
```

- Collection 안의 document를 수정한다.

Parameter|Description
:-:|:-:
query*|업데이트할 document의 조건
update*|적용할 내용
upsert|조건을 만족하는 document가 없다면 새로운 document를 추가 (default : false)
multi|여러 개의 document를 수정 (default : false)
writeConcern|업데이트할 때 필요한 설정값

## Delete

- 여러 document 삭제 : `deleteMany({조건})`

## 기타

- String to date : `Date(string)`
- 