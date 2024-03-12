# nextjs cơ bản

## 1. Tạo mới project

```sh
npx create-next-app@latest nextjs-dashboard --use-pnpm --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example"

pnpm create next-app <app-name>
--ts | --js # using ts | js
--use-npm | --use-pnpm | --use-yarn | --use-bun # Explicitly tell the CLI to bootstrap the app using specific package manger
```

## 2. Cấu trúc project

- /app: Chứa routes, components và logic của app
- /app/lib: Chứa các functions như các utility functions, data fetching functions.
- /app/ui: Chứa các UI components như cards, tables, and forms
- /public: Chứa các static asset như images.
- /scripts: Chứa các script tương tác với database
- Config Files: Các file như next.config.js trong folder root của ứng dụng

## 3. Các lưu ý
