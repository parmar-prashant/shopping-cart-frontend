# Shopping cart

This project uses Vue 3, Tailwind CSS framework with no external libraries.

## Folder Structure

The project has the basic folder structure as of a Vue 3 template with the following sub-directories in the *src* folder:

- *Components Folder*:
  - *Custom*: This folder holds the custom UI components for use across the various components for better code reusability and maintenance e.g. Modal Popup.
  - *Shared*: This folder holds the custom shared components for use within a single component or across different components e.g. Thumbnail viewer.
  - *Skeleton*: This folder holds the custom components such as a header, footer, body and navigation panel for better structuring of the application.
- *Product.vue*: Component used to display product details with media and textual information.
- *ProductImage.vue*: Component used to display product media files in a slider form.
- *ProductDetail.vue*: Component used to display textual information of a product.

## Application component hierarchy

![alt text](https://github.com/parmar-prashant/the-big-phone-store-frontend/blob/main/Component%20Tree.jpeg)

## How to run the project

The following are the ways to run the project:

- Local setup on your system:
  - Install Node and npm.
  - Install Vue CLI using npm.
  - Download the whole project and run the following commands:
    - `npm install`: This will install all the required packages listed in the package.json file.
    - `npm run dev`: This will start the project locally and you can run it in the browser of your choice.
- Project is deployed on Netlify and you can view [here](https://prashant-parmar.netlify.app)

## Note

- As per the requirement, I have kept the Product, ProductImage and ProductDetail directly under the Components folder. In real-time application there will be folder for each main component and child components related to it.
- All the mordern applications are responsive now a days. for this project I have created single component working in responsive manner when viewed either on desktop or mobile device. For real time application we can have mobile component and desktop component for a single feature to have a firm control over design and complex business logic.
