# strapi-provider-upload-aws-s3-cdn-acl-enabled


# Installation

`npm i strapi-provider-upload-aws-s3-cdn-acl-enabled`

## Background

This Strapi upload provider adapts the strapi-provider-upload-aws-s3, bundled with Strapi, to support writes to non-public S3 buckets plus optional download from a CDN endpoint (e.g. AWS Cloudfront).

Inspired by this discussion: https://github.com/strapi/strapi/issues/5868#issuecomment-705200530

This project is essentially the same as https://www.npmjs.com/package/strapi-provider-upload-aws-s3-plus-cdn, but it has public ACL enabled for cdn as well.
## Configuration

Your configuration is passed down to the provider. (e.g: `new AWS.S3(config)`). You can see the complete list of options [here](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#constructor-property)

See the [using a provider](https://strapi.io/documentation/developer-docs/latest/development/plugins/upload.html#using-a-provider) documentation for information on installing and using a provider. And see the [environment variables](https://strapi.io/documentation/developer-docs/latest/setup-deployment-guides/configurations.html#environment-variables) for setting and using environment variables in your configs.

If using a CDN to deliver media files to end users, you can include a `cdnUrl` property, as shown below.

**Example**

`./config/plugins.js`

```js
module.exports = ({ env }) => ({
  // ...
  upload: {
    provider: 'aws-s3-plus-cdn',
    providerOptions: {
      accessKeyId: env('AWS_ACCESS_KEY_ID'),
      secretAccessKey: env('AWS_ACCESS_SECRET'),
      region: env('AWS_REGION'),
      params: {
        Bucket: env('AWS_BUCKET'),
      },
      cdnUrl: env("CDN_URL"), // Optional CDN URL - include protofol and trailing forward slash, e.g. 'https://assets.example.com/'
    },
  },
  // ...
});
```
## Note

Strapi will use the configured S3 bucket for upload and delete operations, but writes the CDN url (if configured) into the database record.

In the event that you need to change the storage backend in the future, to avoid the need to re-upload assets or to write custom queries to update Strapi database records, it is probably best to configure your CDN to use a URL that you control (e.g. use assets.mydomain.com rather than d12345687abc.cloudfront.net). If you need to change the storage backend later, you can simply update your DNS record.
## Resources

- [License](LICENSE)
