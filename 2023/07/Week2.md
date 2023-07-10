# Week 2 (2023-07-10 ~ 2023-07-16)

## Sping MVC2 - 메시지, 국제화

- `messageSource` 에서 직접 메시지를 꺼낼 때에는 다음과 같은 메소드를 이용한다.
```
ms.getMessage(codeName, args, defaultMessage, locale);
```

- 인자를 사용할 때에는 Object 배열 형식으로 전달한다.
```
ms.getMessage("hello", new Object[]{"Spring"}, "defaultMessage", Locale.KOREA);
```

- 타임리프에서는 `#{message.code}` 같은 형식으로 메시지를 불러와 적용할 수 있다.