---
title: "[Next.js] supabase에서 auth 스키마와 public 스키마를 연동하여 사용자 테이블 생성하기"
date: 2024-10-15
layout: single
toc: true
toc_label: "목차"
toc_icon: "align-left"
toc_sticky: true
categories:
  - til
  - next
  - supabase 
tags:
  - supabase
  - auth
author_profile: true
sidebar_main: true
---

## :ledger: supabase에서 auth 스키마와 public 스키마를 연동하여 사용자 테이블 생성하기
보안을 위해 Auth 스키마는 자동 생성된 API에 노출되지 않는ㄴ다. API를 통해 사용자 데이터에 액세스하려면 Public 스키마에서 사용자 테이블을 연동해서 사용할 수 있다. [supabase 공식문서](https://supabase.com/docs/guides/auth/managing-user-data)를 참고해서 진행해봤다.

### :one: public profiles 테이블 생성
수퍼베이스에 접속 후 좌측 사이드바의 `SQL Editor`를 클릭 후 아래의 코드를 입력하고 우측 하단에 `Run`을 클릭한다.

- 아래의 sql문을 입력 후 실행하면, `Table Editor`의 `public` 스키마에서 `profiles` 테이블이 생성된 걸 확인할 수 있다.
- admin 유저를 판별하기 위해 `is_admin`을 추가해주었다.

```sql
create table public.profiles (
  id uuid not null references auth.users on delete cascade,
  first_name text,
  last_name text,
  is_admin boolean DEFAULT false, -- is_admin 추가

  primary key (id)
);

alter table public.profiles enable row level security;
```

### :two: public profiles 트리거 설정
사용자가 회원가입할 때 마다 테이블을 업데이트하려면 `public.profiles` 트리거를 설정해야한다. 다시 수퍼베이스에 접속 후 좌측 사이드바의 `SQL Editor`를 클릭 후 아래의 코드를 입력하고 우측 하단에 `Run`을 클릭한다.

- 트리거 설정 후 회원가입을 하면 자동으로 public 스키마의 profiles가 추가되는걸 확인할 수 있다.


```sql
-- inserts a row into public.profiles
create function public.handle_new_user()
returns trigger
language plpgsql
security definer set search_path = ''
as $$
begin
  insert into public.profiles (id, first_name, last_name)
  values (new.id, new.raw_user_meta_data ->> 'first_name', new.raw_user_meta_data ->> 'last_name');
  return new;
end;
$$;

-- trigger the function every time a user is created
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();

```

### :fire: 마무리
[공식문서 - Setting up Server-Side Auth for Next.js](https://supabase.com/docs/guides/auth/server-side/nextjs?queryGroups=router&router=app)를 통해 Supabase와 Next.js의 Server-Side Auth를 연동하고 이번엔 사용자 테이블을 생성하고 관리하는 방법에 대해 알아보았다. 이러한 과정을 통해 더 안전하고 효율적인 인증 시스템을 구축하는 방법을 배울 수 있었다.

- [https://github.com/rarrit/next-supabase-auth](https://github.com/rarrit/next-supabase-auth)