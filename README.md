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