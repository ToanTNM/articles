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

- Get active path

```js
'use client' // This is a Client Component, which means you can use event listeners and hooks.
...
const pathname = usePathname();
...
<Link
  key={link.name}
  href={link.href}
  className={clsx(
    'flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3',
    {
      'bg-sky-100 text-blue-600': pathname === link.href,
    },
  )}
>
  <LinkIcon className="w-6" />
  <p className="hidden md:block">{link.name}</p>
</Link>
```

- Search process:

  - Capture the user's input.
  - Update the URL with the search params.
  - Keep the URL in sync with the input field.
  - Update the table to reflect the search query.

  - `useSearchParams()` hook vs. the `searchParams` props
    
    - `useSearchParams()`: use in Client Component
    - `searchParams`: use in Server Component
 
  - `useDebouncedCallback` from `use-debounce` to delay running function after specific time

- Update process:
  
  - Create a new dynamic route segment with the invoice id.
  - Read the invoice id from the page params.
  - Fetch the specific invoice from your database.
  - Pre-populate the form with the invoice data.
  - Update the invoice data in your database.
     
    ```js
      export default async function Page({ params }: { params: { id: string } }) {
      const id = params.id;
      // ...
    }
    ```
  - `Dynamic route segments` can created by wrapping a folder's name in square brackets. For example, `[id]`, `[post]` or `[slug]`
    
  - In addition to `searchParams`, page components also accept a prop called `params` which you can use to access the id
    
- Next.js serves as a catch-all for unexpected errors in your route segments in `error.tsx` file

- Use `notFound()` from `next/navigation` to throw (navigate) to `not-found.tsx` page
