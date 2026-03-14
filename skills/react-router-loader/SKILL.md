---
name: react-router-loader
description: React Router v7 の loader パターンのリファレンス。データフェッチ、型安全な useLoaderData、エラーハンドリング、defer/Await による streaming を扱う。
user-invocable: false
---

# React Router Loader

React Router v7 (framework mode) の loader パターン。

## 基本的な loader

```tsx
// app/routes/users.tsx
import type { Route } from "./+types/users";

export async function loader({ params, request }: Route.LoaderArgs) {
  const user = await db.user.findUnique({ where: { id: params.id } });
  if (!user) {
    throw new Response("Not Found", { status: 404 });
  }
  return { user };
}

export default function Users({ loaderData }: Route.ComponentProps) {
  const { user } = loaderData;
  return <div>{user.name}</div>;
}
```

## エラーハンドリング

```tsx
// loader で throw した Response は ErrorBoundary でキャッチ
export function ErrorBoundary({ error }: Route.ErrorBoundaryProps) {
  if (isRouteErrorResponse(error)) {
    return <div>{error.status}: {error.data}</div>;
  }
  return <div>Unexpected error</div>;
}
```

## defer / Await (streaming)

```tsx
import { Suspense } from "react";
import { Await } from "react-router";

export async function loader({ params }: Route.LoaderArgs) {
  const userPromise = db.user.findUnique({ where: { id: params.id } });
  const posts = await db.post.findMany({ where: { userId: params.id } });
  return { posts, userPromise };
}

export default function UserPage({ loaderData }: Route.ComponentProps) {
  const { posts, userPromise } = loaderData;
  return (
    <div>
      <PostList posts={posts} />
      <Suspense fallback={<p>Loading user...</p>}>
        <Await resolve={userPromise}>
          {(user) => <UserProfile user={user} />}
        </Await>
      </Suspense>
    </div>
  );
}
```

## ネストルートでの loader

```tsx
// app/routes/dashboard.tsx (親)
export async function loader({ request }: Route.LoaderArgs) {
  const user = await requireAuth(request);
  return { user };
}

// app/routes/dashboard.settings.tsx (子)
// 親の loader データには useRouteLoaderData でアクセス
import { useRouteLoaderData } from "react-router";

export default function Settings() {
  const dashboardData = useRouteLoaderData("routes/dashboard");
  return <div>{dashboardData?.user.name}</div>;
}
```

## request からの情報取得

```tsx
export async function loader({ request }: Route.LoaderArgs) {
  const url = new URL(request.url);
  const q = url.searchParams.get("q");           // クエリパラメータ
  const cookie = request.headers.get("Cookie");   // ヘッダー
  return { results: await search(q) };
}
```
