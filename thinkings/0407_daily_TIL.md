enum이 말썽이다. userrole은 enum type이 맞는 것 같은데, line_item_category의 expense_nature도 마찬가지.

로컬 DB랑 model 차이가 있어서 그런 것 같은데, 그럼 upgrade 돌리면 변경되어야 하지 않나? 그렇다.
-> 그런데 /aldb인가? alembic 특징인가? model이 변경될 때만 autogenerate을 돌아서 다른 부분이 있으면 python file을 생성한ㄷ.
  그럼 DB에 맞춘 model 수정은 alembic이 돈다고 또는 /aldb를 실행한다고 가능한 상황이 아니지 않은가?
일단 alembic claude.md를 업데이트 해야겠다. 우리 프로젝트는 enum 타입을 varchar로 강제하지 않는다.
오히려 enum type을 지향한다고. 그렇게 썼던 이유는 stage, sandboxap가 varchar를 가지고 있어서 였다. 

명확해졌다. enum type으로 alembic/claude.md(root/claude.md도 확인)를 수정하는 PR 올리고 팀에게 공유하기.

PR이 올라오면 한번 얘기해봐야겠다.

a000에 있는 create expense_nature도 enum type으로 alter하는 코드를 추가해야겠다.
