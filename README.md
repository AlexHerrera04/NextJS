CHAPTER 1

Getting Started
Creating a new project
We recommend using pnpm as your package manager, as it's faster and more efficient than npm or yarn. If you don't have pnpm installed, you can install it globally by running:

Terminal
npm install -g pnpm
To create a Next.js app, open your terminal, cd into the folder you'd like to keep your project, and run the following command:

Terminal
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
This command uses create-next-app, a Command Line Interface (CLI) tool that sets up a Next.js application for you. In the command above, you're also using the --example flag with the starter example for this course.

Exploring the project
Unlike tutorials that have you write code from scratch, much of the code for this course is already written for you. This better reflects real-world development, where you'll likely be working with existing codebases.

Our goal is to help you focus on learning the main features of Next.js, without having to write all the application code.

After installation, open the project in your code editor and navigate to nextjs-dashboard.

Terminal
cd nextjs-dashboard
Let's spend some time exploring the project.

Folder structure
You'll notice that the project has the following folder structure:

Folder structure of the dashboard project, showing the main folders and files: app, public, and config files.
/app: Contains all the routes, components, and logic for your application, this is where you'll be mostly working from.
/app/lib: Contains functions used in your application, such as reusable utility functions and data fetching functions.
/app/ui: Contains all the UI components for your application, such as cards, tables, and forms. To save time, we've pre-styled these components for you.
/public: Contains all the static assets for your application, such as images.
Config Files: You'll also notice config files such as next.config.ts at the root of your application. Most of these files are created and pre-configured when you start a new project using create-next-app. You will not need to modify them in this course.
Feel free to explore these folders, and don't worry if you don't understand everything the code is doing yet.

Placeholder data
When you're building user interfaces, it helps to have some placeholder data. If a database or API is not yet available, you can:

Use placeholder data in JSON format or as JavaScript objects.
Use a 3rd party service like mockAPI.
For this project, we've provided some placeholder data in app/lib/placeholder-data.ts. Each JavaScript object in the file represents a table in your database. For example, for the invoices table:

/app/lib/placeholder-data.ts
const invoices = [
  {
    customer_id: customers[0].id,
    amount: 15795,
    status: 'pending',
    date: '2022-12-06',
  },
  {
    customer_id: customers[1].id,
    amount: 20348,
    status: 'pending',
    date: '2022-11-14',
  },
  // ...
];
In the chapter on setting up your database, you'll use this data to seed your database (populate it with some initial data).

TypeScript
You may also notice most files have a .ts or .tsx suffix. This is because the project is written in TypeScript. We wanted to create a course that reflects the modern web landscape.

It's okay if you don't know TypeScript - we'll provide the TypeScript code snippets when required.

For now, take a look at the /app/lib/definitions.ts file. Here, we manually define the types that will be returned from the database. For example, the invoices table has the following types:

/app/lib/definitions.ts
export type Invoice = {
  id: string;
  customer_id: string;
  amount: number;
  date: string;
  // In TypeScript, this is called a string union type.
  // It means that the "status" property can only be one of the two strings: 'pending' or 'paid'.
  status: 'pending' | 'paid';
};
By using TypeScript, you can ensure you don't accidentally pass the wrong data format to your components or database, like passing a string instead of a number to invoice amount.

If you're a TypeScript developer:

We're manually declaring the data types, but for better type-safety, we recommend Prisma or Drizzle, which automatically generates types based on your database schema.
Next.js detects if your project uses TypeScript and automatically installs the necessary packages and configuration. Next.js also comes with a TypeScript plugin for your code editor, to help with auto-completion and type-safety.
Running the development server
Run pnpm i to install the project's packages.

Terminal
pnpm i
Followed by pnpm dev to start the development server.

Terminal
pnpm dev
pnpm dev starts your Next.js development server on port 3000. Let's check to see if it's working.

Open http://localhost:3000 on your browser. Your home page should look like this, which is intentionally unstyled:



CHAPTER 2

CSS Styling
Currently, your home page doesn't have any styles. Let's look at the different ways you can style your Next.js application.

In this chapter...

Here are the topics we'll cover

How to add a global CSS file to your application.

Two different ways of styling: Tailwind and CSS modules.

How to conditionally add class names with the clsx utility package.

Global styles
If you look inside the /app/ui folder, you'll see a file called global.css. You can use this file to add CSS rules to all the routes in your application - such as CSS reset rules, site-wide styles for HTML elements like links, and more.

You can import global.css in any component in your application, but it's usually good practice to add it to your top-level component. In Next.js, this is the root layout (more on this later).

Add global styles to your application by navigating to /app/layout.tsx and importing the global.css file:

/app/layout.tsx
import '@/app/ui/global.css';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
With the development server still running, save your changes and preview them in the browser. Your home page should now look like this:

Styled page with the logo 'Acme', a description, and login link.
But wait a second, you didn't add any CSS rules, where did the styles come from?

If you take a look inside global.css, you'll notice some @tailwind directives:

/app/ui/global.css
@tailwind base;
@tailwind components;
@tailwind utilities;
Tailwind
Tailwind is a CSS framework that speeds up the development process by allowing you to quickly write utility classes directly in your React code.

In Tailwind, you style elements by adding class names. For example, adding "text-blue-500" will turn the <h1> text blue:

<h1 className="text-blue-500">I'm blue!</h1>
Although the CSS styles are shared globally, each class is singularly applied to each element. This means if you add or delete an element, you don't have to worry about maintaining separate stylesheets, style collisions, or the size of your CSS bundle growing as your application scales.

When you use create-next-app to start a new project, Next.js will ask if you want to use Tailwind. If you select yes, Next.js will automatically install the necessary packages and configure Tailwind in your application.

If you look at /app/page.tsx, you'll see that we're using Tailwind classes in the example.

/app/page.tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
 
export default function Page() {
  return (
    // These are Tailwind classes:
    <main className="flex min-h-screen flex-col p-6">
      <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
    // ...
  )
}
Don't worry if this is your first time using Tailwind. To save time, we've already styled all the components you'll be using.

Let's play with Tailwind! Copy the code below and paste it above the <p> element in /app/page.tsx:

/app/page.tsx
<div
  className="relative w-0 h-0 border-l-[15px] border-r-[15px] border-b-[26px] border-l-transparent border-r-transparent border-b-black"
/>
It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What shape do you see when using the code snippet above?


A
A yellow star

B
A blue triangle

C
A black triangle

D
A red circle
If you prefer writing traditional CSS rules or keeping your styles separate from your JSX - CSS Modules are a great alternative.

CSS Modules
CSS Modules allow you to scope CSS to a component by automatically creating unique class names, so you don't have to worry about style collisions as well.

We'll continue using Tailwind in this course, but let's take a moment to see how you can achieve the same results from the quiz above using CSS modules.

Inside /app/ui, create a new file called home.module.css and add the following CSS rules:

/app/ui/home.module.css
.shape {
  height: 0;
  width: 0;
  border-bottom: 30px solid black;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
}
Then, inside your /app/page.tsx file import the styles and replace the Tailwind class names from the <div> you've added with styles.shape:

/app/page.tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import styles from '@/app/ui/home.module.css';
 
export default function Page() {
  return (
    <main className="flex min-h-screen flex-col p-6">
      <div className={styles.shape} />
    // ...
  )
}
Save your changes and preview them in the browser. You should see the same shape as before.

Tailwind and CSS modules are the two most common ways of styling Next.js applications. Whether you use one or the other is a matter of preference - you can even use both in the same application!

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What is one benefit of using CSS modules?


A
Increase the global scope of CSS classes, making them easier to manage across different files.

B
Provide a way to make CSS classes locally scoped to components by default, reducing the risk of styling conflicts.

C
Automatically compress and minify CSS files for faster page loading.
Using the clsx library to toggle class names
There may be cases where you may need to conditionally style an element based on state or some other condition.

clsx is a library that lets you toggle class names easily. We recommend taking a look at documentation for more details, but here's the basic usage:

Suppose that you want to create an InvoiceStatus component which accepts status. The status can be 'pending' or 'paid'.
If it's 'paid', you want the color to be green. If it's 'pending', you want the color to be gray.
You can use clsx to conditionally apply the classes, like this:

/app/ui/invoices/status.tsx
import clsx from 'clsx';
 
export default function InvoiceStatus({ status }: { status: string }) {
  return (
    <span
      className={clsx(
        'inline-flex items-center rounded-full px-2 py-1 text-sm',
        {
          'bg-gray-100 text-gray-500': status === 'pending',
          'bg-green-500 text-white': status === 'paid',
        },
      )}
    >
    // ...
)}
It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

Search for "clsx" in your code editor, what components use it to conditionally apply class names?


A
`status.tsx` and `pagination.tsx`

B
`table.tsx` and `status.tsx`

C
`nav-links.tsx` and `table.tsx`
Other styling solutions
In addition to the approaches we've discussed, you can also style your Next.js application with:

Sass which allows you to import .css and .scss files.
CSS-in-JS libraries such as styled-jsx, styled-components, and emotion.
Take a look at the CSS documentation for more information.



CHAPTER 3

Optimizing Fonts and Images
In the previous chapter, you learned how to style your Next.js application. Let's continue working on your home page by adding a custom font and a hero image.

In this chapter...

Here are the topics we'll cover

How to add custom fonts with next/font.

How to add images with next/image.

How fonts and images are optimized in Next.js.

Why optimize fonts?
Fonts play a significant role in the design of a website, but using custom fonts in your project can affect performance if the font files need to be fetched and loaded.

Cumulative Layout Shift is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

Mock UI showing initial load of a page, followed by a layout shift as the custom font loads.
Next.js automatically optimizes fonts in the application when you use the next/font module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

How does Next.js optimize fonts?


A
It causes additional network requests which improve performance.

B
It disables all custom fonts.

C
It preloads all fonts at runtime.

D
It hosts font files with other static assets so that there are no additional network requests.
Adding a primary font
Let's add a custom Google font to your application to see how this works.

In your /app/ui folder, create a new file called fonts.ts. You'll use this file to keep the fonts that will be used throughout your application.

Import the Inter font from the next/font/google module - this will be your primary font. Then, specify what subset you'd like to load. In this case, 'latin':

/app/ui/fonts.ts
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
Finally, add the font to the <body> element in /app/layout.tsx:

/app/layout.tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
By adding Inter to the <body> element, the font will be applied throughout your application. Here, you're also adding the Tailwind antialiased class which smooths out the font. It's not necessary to use this class, but it adds a nice touch.

Navigate to your browser, open dev tools and select the body element. You should see Inter and Inter_Fallback are now applied under styles.

Practice: Adding a secondary font
You can also add fonts to specific elements of your application.

Now it's your turn! In your fonts.ts file, import a secondary font called Lusitana and pass it to the <p> element in your /app/page.tsx file. In addition to specifying a subset like you did before, you should also specify different font weights. For example, 400 (normal) and 700 (bold).

Once you're ready, expand the code snippet below to see the solution.

Hints:

If you're unsure what weight options to pass to a font, check the TypeScript errors in your code editor.
Visit the Google Fonts website and search for Lusitana to see what options are available.
See the documentation for adding multiple fonts and the full list of options.
Finally, the <AcmeLogo /> component also uses Lusitana. It was commented out to prevent errors, you can now uncomment it:

/app/page.tsx
// ...
 
export default function Page() {
  return (
    <main className="flex min-h-screen flex-col p-6">
      <div className="flex h-20 shrink-0 items-end rounded-lg bg-blue-500 p-4 md:h-52">
        <AcmeLogo />
        {/* ... */}
      </div>
    </main>
  );
}
Great, you've added two custom fonts to your application! Next, let's add a hero image to the home page.

Why optimize images?
Next.js can serve static assets, like images, under the top-level /public folder. Files inside /public can be referenced in your application.

With regular HTML, you would add an image as follows:

<img
  src="/hero.png"
  alt="Screenshots of the dashboard project showing desktop version"
/>
However, this means you have to manually:

Ensure your image is responsive on different screen sizes.
Specify image sizes for different devices.
Prevent layout shift as the images load.
Lazy load images that are outside the user's viewport.
Image Optimization is a large topic in web development that could be considered a specialization in itself. Instead of manually implementing these optimizations, you can use the next/image component to automatically optimize your images.

The <Image> component
The <Image> Component is an extension of the HTML <img> tag, and comes with automatic image optimization, such as:

Preventing layout shift automatically when images are loading.
Resizing images to avoid shipping large images to devices with a smaller viewport.
Lazy loading images by default (images load as they enter the viewport).
Serving images in modern formats, like WebP and AVIF, when the browser supports it.
Adding the desktop hero image
Let's use the <Image> component. If you look inside the /public folder, you'll see there are two images: hero-desktop.png and hero-mobile.png. These two images are completely different, and they'll be shown depending if the user's device is a desktop or mobile.

In your /app/page.tsx file, import the component from next/image. Then, add the image under the comment:

/app/page.tsx
import AcmeLogo from '@/app/ui/acme-logo';
import { ArrowRightIcon } from '@heroicons/react/24/outline';
import Link from 'next/link';
import { lusitana } from '@/app/ui/fonts';
import Image from 'next/image';
 
export default function Page() {
  return (
    // ...
    <div className="flex items-center justify-center p-6 md:w-3/5 md:px-28 md:py-12">
      {/* Add Hero Images Here */}
      <Image
        src="/hero-desktop.png"
        width={1000}
        height={760}
        className="hidden md:block"
        alt="Screenshots of the dashboard project showing desktop version"
      />
    </div>
    //...
  );
}
Here, you're setting the width to 1000 and height to 760 pixels. It's good practice to set the width and height of your images to avoid layout shift, these should be an aspect ratio identical to the source image. These values are not the size the image is rendered, but instead the size of the actual image file used to understand the aspect ratio.

You'll also notice the class hidden to remove the image from the DOM on mobile screens, and md:block to show the image on desktop screens.

This is what your home page should look like now:

Styled home page with a custom font and hero image
Practice: Adding the mobile hero image
Now it's your turn! Under the image you've just added, add another <Image> component for hero-mobile.png.

The image should have a width of 560 and height of 620 pixels.
It should be shown on mobile screens, and hidden on desktop - you can use dev tools to check if the desktop and mobile images are swapped correctly.
Once you're ready, expand the code snippet below to see the solution.

Great! Your home page now has a custom font and hero images.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

True or False: Images without dimensions and web fonts are common causes of layout shift.


A
True

B
False
Recommended reading
There's a lot more to learn about these topics, including optimizing remote images and using local font files. If you'd like to dive deeper into fonts and images, see:

Image Optimization Docs
Font Optimization Docs
Improving Web Performance with Images (MDN)
Web Fonts (MDN)
How Core Web Vitals Affect SEO
How Google handles JavaScript throughout the indexing process






CHAPTER 4

Creating Layouts and Pages
So far, your application only has a home page. Let's learn how you can create more routes with layouts and pages.

In this chapter...

Here are the topics we'll cover

Create the dashboard routes using file-system routing.

Understand the role of folders and files when creating new route segments.

Create a nested layout that can be shared between multiple dashboard pages.

Understand what colocation, partial rendering, and the root layout are.

Nested routing
Next.js uses file-system routing where folders are used to create nested routes. Each folder represents a route segment that maps to a URL segment.

Diagram showing how folders map to URL segments
You can create separate UIs for each route using layout.tsx and page.tsx files.

page.tsx is a special Next.js file that exports a React component, and it's required for the route to be accessible. In your application, you already have a page file: /app/page.tsx - this is the home page associated with the route /.

To create a nested route, you can nest folders inside each other and add page.tsx files inside them. For example:

Diagram showing how adding a folder called dashboard creates a new route '/dashboard'
/app/dashboard/page.tsx is associated with the /dashboard path. Let's create the page to see how it works!

Creating the dashboard page
Create a new folder called dashboard inside /app. Then, create a new page.tsx file inside the dashboard folder with the following content:

/app/dashboard/page.tsx
export default function Page() {
  return <p>Dashboard Page</p>;
}
Now, make sure that the development server is running and visit http://localhost:3000/dashboard. You should see the "Dashboard Page" text.

This is how you can create different pages in Next.js: create a new route segment using a folder, and add a page file inside it.

By having a special name for page files, Next.js allows you to colocate UI components, test files, and other related code with your routes. Only the content inside the page file will be publicly accessible. For example, the /ui and /lib folders are colocated inside the /app folder along with your routes.

Practice: Creating the dashboard pages
Let's practice creating more routes. In your dashboard, create two more pages:

Customers Page: The page should be accessible on http://localhost:3000/dashboard/customers. For now, it should return a <p>Customers Page</p> element.
Invoices Page: The invoices page should be accessible on http://localhost:3000/dashboard/invoices. For now, also return a <p>Invoices Page</p> element.
Spend some time tackling this exercise, and when you're ready, expand the toggle below for the solution:

Creating the dashboard layout
Dashboards have some sort of navigation that is shared across multiple pages. In Next.js, you can use a special layout.tsx file to create UI that is shared between multiple pages. Let's create a layout for the dashboard pages!

Inside the /dashboard folder, add a new file called layout.tsx and paste the following code:

/app/dashboard/layout.tsx
import SideNav from '@/app/ui/dashboard/sidenav';
 
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <div className="flex h-screen flex-col md:flex-row md:overflow-hidden">
      <div className="w-full flex-none md:w-64">
        <SideNav />
      </div>
      <div className="grow p-6 md:overflow-y-auto md:p-12">{children}</div>
    </div>
  );
}
A few things are going on in this code, so let's break it down:

First, you're importing the <SideNav /> component into your layout. Any components you import into this file will be part of the layout.

The <Layout /> component receives a children prop. This child can either be a page or another layout. In your case, the pages inside /dashboard will automatically be nested inside a <Layout /> like so:

Folder structure with dashboard layout nesting the dashboard pages as children
Check that everything is working correctly by saving your changes and checking your localhost. You should see the following:

Dashboard page with a sidenav and a main content area
One benefit of using layouts in Next.js is that on navigation, only the page components update while the layout won't re-render. This is called partial rendering which preserves client-side React state in the layout when transitioning between pages.

Folder structure showing the dashboard layout nesting the dashboard pages, but only the pages UI swap on navigation
Root layout
In Chapter 3, you imported the Inter font into another layout: /app/layout.tsx. As a reminder:

/app/layout.tsx
import '@/app/ui/global.css';
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
This is called a root layout and is required in every Next.js application. Any UI you add to the root layout will be shared across all pages in your application. You can use the root layout to modify your <html> and <body> tags, and add metadata (you'll learn more about metadata in a later chapter).

Since the new layout you've just created (/app/dashboard/layout.tsx) is unique to the dashboard pages, you don't need to add any UI to the root layout above.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What is the purpose of the layout file in Next.js?


A
To act as a global error handler

B
To fetch data and manage state across the entire application

C
To share UI across multiple pages

D
To act as the entry point for the entire application
4
You've Completed Chapter 4
Nice, the dashboard app is slowly starting to come together.


CHAPTER 5

Navigating Between Pages
In the previous chapter, you created the dashboard layout and pages. Now, let's add some links to allow users to navigate between the dashboard routes.

In this chapter...

Here are the topics we'll cover


How to use the next/link component.

How to show an active link with the usePathname() hook.

How navigation works in Next.js.

Why optimize navigation?
To link between pages, you'd traditionally use the <a> HTML element. At the moment, the sidebar links use <a> elements, but notice what happens when you navigate between the home, invoices, and customers pages on your browser.

Did you see it?

There's a full page refresh on each page navigation!

The <Link> component
In Next.js, you can use the <Link /> Component to link between pages in your application. <Link> allows you to do client-side navigation with JavaScript.

To use the <Link /> component, open /app/ui/dashboard/nav-links.tsx, and import the Link component from next/link. Then, replace the <a> tag with <Link>:

/app/ui/dashboard/nav-links.tsx
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
 
// ...
 
export default function NavLinks() {
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
          <Link
            key={link.name}
            href={link.href}
            className="flex h-[48px] grow items-center justify-center gap-2 rounded-md bg-gray-50 p-3 text-sm font-medium hover:bg-sky-100 hover:text-blue-600 md:flex-none md:justify-start md:p-2 md:px-3"
          >
            <LinkIcon className="w-6" />
            <p className="hidden md:block">{link.name}</p>
          </Link>
        );
      })}
    </>
  );
}
As you can see, the Link component is similar to using <a> tags, but instead of <a href="…">, you use <Link href="…">.

Save your changes and check to see if it works in your localhost. You should now be able to navigate between the pages without seeing a full refresh. Although parts of your application are rendered on the server, there's no full page refresh, making it feel like a native web app. Why is that?

Automatic code-splitting and prefetching
To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on the initial page load.

Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work. This is also less code for the browser to parse, which makes your application faster.

Furthermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

Learn more about how navigation works.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What does Next.js do when a <Link> component appears in the browser’s viewport in a production environment?


A
Downloads additional CSS

B
Preloads images

C
Prefetches the code for the linked route

D
Enables lazy loading for the linked route
Pattern: Showing active links
A common UI pattern is to show an active link to indicate to the user what page they are currently on. To do this, you need to get the user's current path from the URL. Next.js provides a hook called usePathname() that you can use to check the path and implement this pattern.

Since usePathname() is a React hook, you'll need to turn nav-links.tsx into a Client Component. Add React's "use client" directive to the top of the file, then import usePathname() from next/navigation:

/app/ui/dashboard/nav-links.tsx
'use client';
 
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
 
// ...
Next, assign the path to a variable called pathname inside your <NavLinks /> component:

/app/ui/dashboard/nav-links.tsx
export default function NavLinks() {
  const pathname = usePathname();
  // ...
}
Note: nav-links.tsx is not a special file for Next.js — it can be named whatever you want. If you rename it, ensure that you update the import statements accordingly.

You can use the clsx library introduced in the chapter on CSS styling to conditionally apply class names when the link is active. When link.href matches the pathname, the link should be displayed with blue text and a light blue background.

Here's the final code for nav-links.tsx:

/app/ui/dashboard/nav-links.tsx
'use client';
 
import {
  UserGroupIcon,
  HomeIcon,
  DocumentDuplicateIcon,
} from '@heroicons/react/24/outline';
import Link from 'next/link';
import { usePathname } from 'next/navigation';
import clsx from 'clsx';
 
// ...
 
export default function NavLinks() {
  const pathname = usePathname();
 
  return (
    <>
      {links.map((link) => {
        const LinkIcon = link.icon;
        return (
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
        );
      })}
    </>
  );
}
Save and check your localhost. You should now see the active link highlighted in blue.

5
You've Completed Chapter 5
You've learned how to link between pages and leverage client-side navigation in Next.js.



CHAPTER 6

Setting Up Your Database
Before you can continue working on your dashboard, you'll need some data. In this chapter, you'll be setting up a PostgreSQL database from one of Vercel's marketplace integrations. If you're already familiar with PostgreSQL and would prefer to use your own database provider, you can skip this chapter and set it up on your own. Otherwise, let's continue!

In this chapter...

Here are the topics we'll cover


Push your project to GitHub.


Set up a Vercel account and link your GitHub repo for instant previews and deployments.

Create and link your project to a Postgres database.

Seed the database with initial data.

Create a GitHub repository
To start, let's push your repository to GitHub if you haven't already. This will make it easier to set up your database and deploy.

If you need help setting up your repository, take a look at this guide on GitHub.

Good to know:

You can also use other git providers like GitLab or Bitbucket.
If you're new to GitHub, we recommend the GitHub Desktop App for a simplified development workflow.
Create a Vercel account
Visit vercel.com/signup to create an account. Choose the free "hobby" plan. Select Continue with GitHub to connect your GitHub and Vercel accounts.

Connect and deploy your project
Next, you'll be taken to this screen where you can select and import the GitHub repository you've just created:

Screenshot of Vercel Dashboard, showing the import project screen with a list of the user's GitHub Repositories
Name your project and click Deploy.

Deployment screen showing the project name field and a deploy button
Hooray! 🎉 Your project is now deployed.

Project overview screen showing the project name, domain, and deployment status
By connecting your GitHub repository, whenever you push changes to your main branch, Vercel will automatically redeploy your application with no configuration needed. When opening pull requests, you'll also have instant preview URLs which allow you to catch deployment errors early and share a preview of your project with team members for feedback.

Create a Postgres database
Next, to set up a database, click Continue to Dashboard and select the Storage tab from your project dashboard. Select Create Database. Depending on when your Vercel account was created, you may see options like Neon or Supabase. Choose your preferred provider and click Continue.

Connect Store screen showing the Postgres option along with KV, Blob and Edge Config
Choose your region and storage plan, if required. The default region for all Vercel projects is Washington D.C (iad1), and we recommend choosing this if available to reduce latency for data requests.

Database creation modal showing the database name and region
Once connected, navigate to the .env.local tab, click Show secret and Copy Snippet. Make sure you reveal the secrets before copying them.

The .env.local tab showing the hidden database secrets
Navigate to your code editor and rename the .env.example file to .env. Paste in the copied contents from Vercel.

Important: Go to your .gitignore file and make sure .env is in the ignored files to prevent your database secrets from being exposed when you push to GitHub.

Seed your database
Now that your database has been created, let's seed it with some initial data.

We've included an API you can access in the browser, which will run a seed script to populate the database with an initial set of data.

The script uses SQL to create the tables, and the data from placeholder-data.ts file to populate them after they've been created.

Ensure your local development server is running with pnpm run dev and navigate to localhost:3000/seed in your browser. When finished, you will see a message "Database seeded successfully" in the browser. Once completed, you can delete this file.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What is 'seeding' in the context of databases?


A
Deleting all data in the database

B
Importing the schema of a database

C
Populating the database with an initial set of data

D
Creating relationships between tables in a database
Troubleshooting:

Make sure to reveal your database secrets before copying it into your .env file.
The script uses bcrypt to hash the user's password, if bcrypt isn't compatible with your environment, you can update the script to use bcryptjs instead.
If you run into any issues while seeding your database and want to run the script again, you can drop any existing tables by running DROP TABLE tablename in your database query interface. See the executing queries section below for more details. But be careful, this command will delete the tables and all their data. It's ok to do this with your example app since you're working with placeholder data, but you shouldn't run this command in a production app.
Executing queries
Let's execute a query to make sure everything is working as expected. We'll use another Router Handler, app/query/route.ts, to query the database. Inside this file, you'll find a listInvoices() function that has the following SQL query.

SELECT invoices.amount, customers.name
FROM invoices
JOIN customers ON invoices.customer_id = customers.id
WHERE invoices.amount = 666;
Uncomment the file, remove the Response.json() block, and navigate to localhost:3000/query in your browser. You should see that an invoice amount and name is returned.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

Which customer does this invoice belong to?


A
Lee Robinson

B
Evil Rabbit

C
Delba de Oliveira

D
Michael Novotny
6
You've Completed Chapter 6
With your database now set up and integrated, you can continue building your application.



CHAPTER 7

Fetching Data

7

Chapter 7

Fetching Data
Now that you've created and seeded your database, let's discuss the different ways you can fetch data for your application, and build out your dashboard overview page.

In this chapter...

Here are the topics we'll cover

Learn about some approaches to fetching data: APIs, ORMs, SQL, etc.

How Server Components can help you access back-end resources more securely.

What network waterfalls are.

How to implement parallel data fetching using a JavaScript Pattern.

Choosing how to fetch data
API layer
APIs are an intermediary layer between your application code and database. There are a few cases where you might use an API:

If you're using third-party services that provide an API.
If you're fetching data from the client, you want to have an API layer that runs on the server to avoid exposing your database secrets to the client.
In Next.js, you can create API endpoints using Route Handlers.

Database queries
When you're creating a full-stack application, you'll also need to write logic to interact with your database. For relational databases like Postgres, you can do this with SQL or with an ORM.

There are a few cases where you have to write database queries:

When creating your API endpoints, you need to write logic to interact with your database.
If you are using React Server Components (fetching data on the server), you can skip the API layer, and query your database directly without risking exposing your database secrets to the client.
It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

In which of these scenarios should you not query your database directly?


A
When you're fetching data on the client

B
When you're fetching data on the server

C
When you're creating your own API layer to interact with your database
Let's learn more about React Server Components.

Using Server Components to fetch data
By default, Next.js applications use React Server Components. Fetching data with Server Components is a relatively new approach and there are a few benefits of using them:

Server Components support JavaScript Promises, providing a solution for asynchronous tasks like data fetching natively. You can use async/await syntax without needing useEffect, useState or other data fetching libraries.
Server Components run on the server, so you can keep expensive data fetches and logic on the server, only sending the result to the client.
Since Server Components run on the server, you can query the database directly without an additional API layer. This saves you from writing and maintaining additional code.
It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What's one advantage of using React Server Components to fetch data?


A
They automatically protect you from SQL injection.

B
They allow you to query the database directly from the server without an additional API layer.

C
They require you to use an API layer and create endpoints.
Using SQL
For your dashboard application, you'll write database queries using the postgres.js library and SQL. There are a few reasons why we'll be using SQL:

SQL is the industry standard for querying relational databases (e.g. ORMs generate SQL under the hood).
Having a basic understanding of SQL can help you understand the fundamentals of relational databases, allowing you to apply your knowledge to other tools.
SQL is versatile, allowing you to fetch and manipulate specific data.
The postgres.js library provides protection against SQL injections.
Don't worry if you haven't used SQL before - we have provided the queries for you.

Go to /app/lib/data.ts. Here you'll see that we're using postgres. The sql function allows you to query your database:

/app/lib/data.ts
import postgres from 'postgres';
 
const sql = postgres(process.env.POSTGRES_URL!, { ssl: 'require' });
 
// ...
You can call sql anywhere on the server, like a Server Component. But to allow you to navigate the components more easily, we've kept all the data queries in the data.ts file, and you can import them into the components.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

What does SQL allow you to do in terms of fetching data?


A
Fetch all your data indiscriminately

B
Fetch and manipulate specific data

C
Automatically cache data for better performance

D
Change the database schema on the fly
Note: If you used your own database provider in Chapter 6, you'll need to update the database queries to work with your provider. You can find the queries in /app/lib/data.ts.

Fetching data for the dashboard overview page
Now that you understand different ways of fetching data, let's fetch data for the dashboard overview page. Navigate to /app/dashboard/page.tsx, paste the following code, and spend some time exploring it:

/app/dashboard/page.tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
 
export default async function Page() {
  return (
    <main>
      <h1 className={`${lusitana.className} mb-4 text-xl md:text-2xl`}>
        Dashboard
      </h1>
      <div className="grid gap-6 sm:grid-cols-2 lg:grid-cols-4">
        {/* <Card title="Collected" value={totalPaidInvoices} type="collected" /> */}
        {/* <Card title="Pending" value={totalPendingInvoices} type="pending" /> */}
        {/* <Card title="Total Invoices" value={numberOfInvoices} type="invoices" /> */}
        {/* <Card
          title="Total Customers"
          value={numberOfCustomers}
          type="customers"
        /> */}
      </div>
      <div className="mt-6 grid grid-cols-1 gap-6 md:grid-cols-4 lg:grid-cols-8">
        {/* <RevenueChart revenue={revenue}  /> */}
        {/* <LatestInvoices latestInvoices={latestInvoices} /> */}
      </div>
    </main>
  );
}
The code above is intentionally commented out. We will now begin to example each piece.

The page is an async server component. This allows you to use await to fetch data.
There are also 3 components which receive data: <Card>, <RevenueChart>, and <LatestInvoices>. They are currently commented out and not yet implemented.
Fetching data for <RevenueChart/>
To fetch data for the <RevenueChart/> component, import the fetchRevenue function from data.ts and call it inside your component:

/app/dashboard/page.tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
import { fetchRevenue } from '@/app/lib/data';
 
export default async function Page() {
  const revenue = await fetchRevenue();
  // ...
}
Next, let's do the following:

Uncomment the <RevenueChart/> component.
Navigate to the component file (/app/ui/dashboard/revenue-chart.tsx) and uncomment the code inside it.
Check localhost:3000/dashboard and you should see a chart that uses revenue data.
Revenue chart showing the total revenue for the last 12 months
Let's continue importing more data and displaying it on the dashboard.

Fetching data for <LatestInvoices/>
For the <LatestInvoices /> component, we need to get the latest 5 invoices, sorted by date.

You could fetch all the invoices and sort through them using JavaScript. This isn't a problem as our data is small, but as your application grows, it can significantly increase the amount of data transferred on each request and the JavaScript required to sort through it.

Instead of sorting through the latest invoices in-memory, you can use an SQL query to fetch only the last 5 invoices. For example, this is the SQL query from your data.ts file:

/app/lib/data.ts
// Fetch the last 5 invoices, sorted by date
const data = await sql<LatestInvoiceRaw[]>`
  SELECT invoices.amount, customers.name, customers.image_url, customers.email
  FROM invoices
  JOIN customers ON invoices.customer_id = customers.id
  ORDER BY invoices.date DESC
  LIMIT 5`;
In your page, import the fetchLatestInvoices function:

/app/dashboard/page.tsx
import { Card } from '@/app/ui/dashboard/cards';
import RevenueChart from '@/app/ui/dashboard/revenue-chart';
import LatestInvoices from '@/app/ui/dashboard/latest-invoices';
import { lusitana } from '@/app/ui/fonts';
import { fetchRevenue, fetchLatestInvoices } from '@/app/lib/data';
 
export default async function Page() {
  const revenue = await fetchRevenue();
  const latestInvoices = await fetchLatestInvoices();
  // ...
}
Then, uncomment the <LatestInvoices /> component. You will also need to uncomment the relevant code in the <LatestInvoices /> component itself, located at /app/ui/dashboard/latest-invoices.

If you visit your localhost, you should see that only the last 5 are returned from the database. Hopefully, you're beginning to see the advantages of querying your database directly!

Latest invoices component alongside the revenue chart
Practice: Fetch data for the <Card> components
Now it's your turn to fetch data for the <Card> components. The cards will display the following data:

Total amount of invoices collected.
Total amount of invoices pending.
Total number of invoices.
Total number of customers.
Again, you might be tempted to fetch all the invoices and customers, and use JavaScript to manipulate the data. For example, you could use Array.length to get the total number of invoices and customers:

const totalInvoices = allInvoices.length;
const totalCustomers = allCustomers.length;
But with SQL, you can fetch only the data you need. It's a little longer than using Array.length, but it means less data needs to be transferred during the request. This is the SQL alternative:

/app/lib/data.ts
const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
The function you will need to import is called fetchCardData. You will need to destructure the values returned from the function.

Hint:

Check the card components to see what data they need.
Check the data.ts file to see what the function returns.
Once you're ready, expand the toggle below for the final code:

Great! You've now fetched all the data for the dashboard overview page. Your page should look like this:

Dashboard page with all the data fetched
However... there are two things you need to be aware of:

The data requests are unintentionally blocking each other, creating a request waterfall.
By default, Next.js prerenders routes to improve performance, this is called Static Rendering. So if your data changes, it won't be reflected in your dashboard.
Let's discuss number 1 in this chapter, then look into detail at number 2 in the next chapter.

What are request waterfalls?
A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

Diagram showing time with sequential data fetching and parallel data fetching
For example, we need to wait for fetchRevenue() to execute before fetchLatestInvoices() can start running, and so on.

/app/dashboard/page.tsx
const revenue = await fetchRevenue();
const latestInvoices = await fetchLatestInvoices(); // wait for fetchRevenue() to finish
const {
  numberOfInvoices,
  numberOfCustomers,
  totalPaidInvoices,
  totalPendingInvoices,
} = await fetchCardData(); // wait for fetchLatestInvoices() to finish
This pattern is not necessarily bad. There may be cases where you want waterfalls because you want a condition to be satisfied before you make the next request. For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

However, this behavior can also be unintentional and impact performance.

It’s time to take a quiz!
Test your knowledge and see what you’ve just learned.

When might you want to use a waterfall pattern?


A
To satisfy a condition before making the next request

B
To make all requests simultaneously

C
To reduce the server load by doing one fetch at a time
Parallel data fetching
A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.

In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time. For example, in data.ts, we're using Promise.all() in the fetchCardData() function:

/app/lib/data.ts
export async function fetchCardData() {
  try {
    const invoiceCountPromise = sql`SELECT COUNT(*) FROM invoices`;
    const customerCountPromise = sql`SELECT COUNT(*) FROM customers`;
    const invoiceStatusPromise = sql`SELECT
         SUM(CASE WHEN status = 'paid' THEN amount ELSE 0 END) AS "paid",
         SUM(CASE WHEN status = 'pending' THEN amount ELSE 0 END) AS "pending"
         FROM invoices`;
 
    const data = await Promise.all([
      invoiceCountPromise,
      customerCountPromise,
      invoiceStatusPromise,
    ]);
    // ...
  }
}
By using this pattern, you can:

Start executing all data fetches at the same time, which is faster than waiting for each request to complete in a waterfall.
Use a native JavaScript pattern that can be applied to any library or framework.
However, there is one disadvantage of relying only on this JavaScript pattern: what happens if one data request is slower than all the others? Let's find out more in the next chapter.

7
You've Completed Chapter 7
You've learned about some of the different ways to fetch data in Next.js.