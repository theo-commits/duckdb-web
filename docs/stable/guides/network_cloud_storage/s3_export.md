---
layout: docu
redirect_from:
- /docs/guides/import/s3_export
- /docs/guides/import/s3_export/
- /docs/guides/network_cloud_storage/s3_export
title: S3 Parquet Export
---

To write a Parquet file to S3, the [`httpfs` extension]({% link docs/stable/core_extensions/httpfs/overview.md %}) is required. This can be installed using the `INSTALL` SQL command. This only needs to be run once.

```sql
INSTALL httpfs;
```

To load the `httpfs` extension for usage, use the `LOAD` SQL command:

```sql
LOAD httpfs;
```

After loading the `httpfs` extension, set up the credentials to write data. Note that the `region` parameter should match the region of the bucket you want to access.

```sql
CREATE SECRET (
    TYPE s3,
    KEY_ID '⟨AKIAIOSFODNN7EXAMPLE⟩',
    SECRET '⟨wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY⟩',
    REGION '⟨us-east-1⟩'
);
```

> Tip If you get an IO Error (`Connection error for HTTP HEAD`), configure the endpoint explicitly via `ENDPOINT 's3.⟨your_region⟩.amazonaws.com'`{:.language-sql .highlight}.

Alternatively, use the [`aws` extension]({% link docs/stable/core_extensions/aws.md %}) to retrieve the credentials automatically:

```sql
CREATE SECRET (
    TYPE s3,
    PROVIDER credential_chain
);
```

After the `httpfs` extension is set up and the S3 credentials are correctly configured, Parquet files can be written to S3 using the following command:

```sql
COPY ⟨table_name⟩ TO 's3://⟨s3-bucket⟩/⟨filename⟩.parquet';
```

Similarly, Google Cloud Storage (GCS) is supported through the Interoperability API.
You need to create [HMAC keys](https://console.cloud.google.com/storage/settings;tab=interoperability) and provide the credentials as follows:

```sql
CREATE SECRET (
    TYPE gcs,
    KEY_ID '⟨AKIAIOSFODNN7EXAMPLE⟩',
    SECRET '⟨wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY⟩'
);
```

After setting up the GCS credentials, you can export using:

```sql
COPY ⟨table_name⟩ TO 'gs://⟨gcs_bucket⟩/⟨filename⟩.parquet';
```
