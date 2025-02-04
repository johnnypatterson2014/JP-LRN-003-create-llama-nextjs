# JP-LRN-003-create-llama-nextjs
https://www.npmjs.com/package/create-llama

Steps
 -      npx create-llama@latest // run the example project template builder (choose nextjs instead of fastapi)
 -      npm install // install the modules and packages
 -      run wsl (windows shell for linux) since the generate script won't work in a regular cmd shell. You only need to do this once.
 -      npm run generate // will convert any files in the ./data/ folder into embeddings and load into vector store. You only need to do this once, or anytime you add new files to the /data folder
 -      npm run dev  // run the dev server
 -      http://localhost:3000/     // dev server should be up and running


Deployed to Vercel:
 - https://jp-lrn-003-create-llama-nextjs.vercel.app/
 - Note: you have to upload the .env file to create the required env variables needed by the app. 


# -------------------------------------------------------------------------------

Route (app)                              Size     First Load JS
┌ ○ /                                    525 kB          630 kB
├ ○ /_not-found                          986 B           107 kB
├ ƒ /api/chat                            151 B           106 kB
├ ƒ /api/chat/config                     151 B           106 kB
├ ƒ /api/chat/config/llamacloud          151 B           106 kB
├ ƒ /api/chat/upload                     151 B           106 kB
├ ƒ /api/files/[...slug]                 151 B           106 kB
└ ƒ /api/sandbox                         151 B           106 kB
+ First Load JS shared by all            106 kB
  ├ chunks/4bd1b696-d81b40d3908bb6c3.js  53 kB
  ├ chunks/517-581dde426bb2d8c2.js       50.6 kB
  └ other shared chunks (total)          2.01 kB


# -------------------------------------------------------------------------------
# Readme file generated from template

This is a [LlamaIndex](https://www.llamaindex.ai/) project using [Next.js](https://nextjs.org/) bootstrapped with [`create-llama`](https://github.com/run-llama/LlamaIndexTS/tree/main/packages/create-llama).

## Getting Started

First, install the dependencies:

```
npm install
```

Second, generate the embeddings of the documents in the `./data` directory:

```
npm run generate
```

Third, run the development server:

```
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `app/page.tsx`. The page auto-updates as you edit the file.

This project uses [`next/font`](https://nextjs.org/docs/basic-features/font-optimization) to automatically optimize and load Inter, a custom Google Font.

## Using Docker

1. Build an image for the Next.js app:

```
docker build -t <your_app_image_name> .
```

2. Generate embeddings:

Parse the data and generate the vector embeddings if the `./data` folder exists - otherwise, skip this step:

```
docker run \
  --rm \
  -v $(pwd)/.env:/app/.env \ # Use ENV variables and configuration from your file-system
  -v $(pwd)/config:/app/config \
  -v $(pwd)/data:/app/data \
  -v $(pwd)/cache:/app/cache \ # Use your file system to store the vector database
  <your_app_image_name> \
  npm run generate
```

3. Start the app:

```
docker run \
  --rm \
  -v $(pwd)/.env:/app/.env \ # Use ENV variables and configuration from your file-system
  -v $(pwd)/config:/app/config \
  -v $(pwd)/cache:/app/cache \ # Use your file system to store gea vector database
  -p 3000:3000 \
  <your_app_image_name>
```

## Learn More

To learn more about LlamaIndex, take a look at the following resources:

- [LlamaIndex Documentation](https://docs.llamaindex.ai) - learn about LlamaIndex (Python features).
- [LlamaIndexTS Documentation](https://ts.llamaindex.ai) - learn about LlamaIndex (Typescript features).

You can check out [the LlamaIndexTS GitHub repository](https://github.com/run-llama/LlamaIndexTS) - your feedback and contributions are welcome!


