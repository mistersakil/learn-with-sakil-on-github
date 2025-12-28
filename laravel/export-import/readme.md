# Large Lead Import & Export Architecture (Laravel CRM)

For Laravel CRMs that handle **large lead imports and exports**, preventing timeouts and memory exhaustion is critical. The recommended approach is to **process data in manageable chunks** and **offload heavy operations to background queues**.

This document outlines the architecture, implementation steps, and best practices for building a scalable and reliable import/export system.

---

## Core Architecture & Implementation

The solution combines two key technical strategies:

| Strategy            | Purpose                                                                 | Key Implementation                                                                 |
|---------------------|-------------------------------------------------------------------------|------------------------------------------------------------------------------------|
| Chunk Processing    | Prevents server memory overload and request timeouts by processing data in batches | Use `WithChunkReading` and `WithBatchInserts` from the `maatwebsite/excel` package |
| Queue Jobs          | Moves long-running import/export tasks to the background                | Create Laravel Jobs such as `ExportLeadsJob` or `ImportLeadsJob`                   |
| User Notification   | Keeps users informed when processing is complete                        | Send email or in-app notifications with a download link                            |

The most efficient tool for this use case is **`maatwebsite/excel`**, which is purpose-built for large datasets and integrates seamlessly with Laravel queues.

---

## Implementation Steps

### 1. Install and Configure Laravel Excel

Install the package via Composer:

```bash
composer require maatwebsite/excel
```

Ensure queues are properly configured in your application.

### 2. Create an Export Job

Install the package via Composer:

```bash
composer php artisan make:job ExportLeadsJob
```

#### Export Job Guidelines

* Implement all heavy logic inside the job’s handle() method.
* Structure exports using chunk-based queries.
* The export class should implement:

  * FromQuery
  * WithChunkReading
  * ShouldQueue

This ensures efficient memory usage and asynchronous execution.

### 3. Create an Import Job

Create the import job:

```bash
php artisan make:job ImportLeadsJob
```

#### Required Import Concerns

* WithChunkReading
* WithBatchInserts
* WithValidation
* WithUpserts

#### Recommended Configuration

```bash
chunkSize(): 100
batchSize(): 50
```

##### Data Integrity Best Practices

* Implement uniqueBy() (e.g., using email) to avoid duplicate leads.
* Validate each row before database insertion.

### 4. Integrate with Inertia / React Frontend

1. Add an Import or Export button in your React component.

2. Trigger a POST request via Inertia to a Laravel controller.

3. Dispatch the job from the controller:

```bash
ExportLeadsJob::dispatch(auth()->user());
```

4. Immediately return a response to keep the UI responsive.
5. Update the UI using:
    * Email notifications
    * In-app notifications
    * Broadcasting (WebSockets) or polling

### 5. Additional Optimizations and Considerations

#### Server Configuration

##### For extremely large files, consider adjusting:

* max_execution_time

* memory_limit

These should be used as supportive optimizations, not primary solutions.

#### Lead Data Management Considerations

When importing leads, enhance CRM data quality with the following practices:

##### Data Source Tagging

* Add a dedicated lead_source value, for example:

```bash
Excel Import - 2025-12-29
```

* Helps track the origin of imported data.

##### Assignment Rules

* Automatically assign leads based on:

  * Geography

  * Industry

  * Company size

##### Campaign Tracking

* Group imported leads under a campaign or list.

* Enables performance tracking and reporting.

### 6. Export Process Flow

* User initiates export from the UI
* Controller dispatches ExportLeadsJob
* Job processes records in chunks
* Export file is generated in background storage
* User receives a notification with the download link

![Export Process Flow](/_images/laravel-export-import.png)

### 7.Summary

By combining chunk processing, queued jobs, and user notifications, large lead imports and exports can be handled safely and efficiently in Laravel. The maatwebsite/excel package provides the necessary tools to build a scalable and reliable solution without browser timeouts or memory issues.
