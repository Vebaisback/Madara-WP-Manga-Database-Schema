# Madara WordPress Theme: Database Structure for Manga Series and Chapters

The Madara theme manages manga series and chapters using WordPress’s standard database schema combined with Custom Post Types (CPTs). It typically bundles the core **WP-Manga** plugin to ensure all manga content remains intact even if you switch themes. Below is an English-language Markdown overview suitable for a GitHub repository.

---

## Custom Post Types (CPTs)

Madara separates manga content from regular posts and pages by defining two CPTs:

### Manga Series
- **Post Type**: `wp-manga`  
- **What it stores**:  
  - **Title**: Name of the manga series  
  - **Content**: Description or summary of the series  
  - **Featured Image**: Cover image for the series  

### Manga Chapter
- **Post Type**: `wp-chapter`  
- **What it stores**:  
  - **Title**: Chapter name or number (e.g., “Chapter 1” or “A New Beginning”)  
  - **Content**: Notes or text related to the chapter  
- **Link to series**:  
  - Uses `post_parent` to reference the parent `wp-manga` post ID  

---

## Database Tables and Relationships

Madara leverages the built-in WordPress tables rather than creating new ones, ensuring full compatibility with all WP functions.

### `wp_posts`
- Stores both **Manga Series** and **Manga Chapters**  
- **post_type** differentiates regular posts/pages from `wp-manga` and `wp-chapter`  
- **post_parent** links chapters to their parent series  

### `wp_postmeta`
- Holds all additional metadata for series and chapters  
- Each row is tied to a post by `post_id`  
- Meta keys use `_wp_manga_` prefixes  

#### Example Series Meta Keys
- `_wp_manga_status` – Series status (Ongoing, Completed, Dropped)  
- `_wp_manga_author` – Author(s)  
- `_wp_manga_artist` – Artist(s)  
- `_wp_manga_release` – Release year  
- `_wp_manga_views` – Total view count  
- `_wp_manga_rating` – Average rating and vote count  

#### Example Chapter Meta Keys
- `_wp_manga_chapter_name_extend` – Extended chapter title info (e.g., “v2”)  
- `_wp_chapter_type` – Media type (images or video)  
- `_wp_manga_storage` – Storage location (local, Amazon S3, Imgur, etc.)  

---

## Taxonomies (Categories & Tags)

To classify manga by genre, tags, etc., Madara uses WordPress’s native taxonomy system:

- **Tables**: `wp_terms`, `wp_term_taxonomy`, `wp_term_relationships`  
- **Custom Taxonomies**:  
  - `manga-genre`  
  - `manga-tag`  
- Relationships between series and taxonomy terms are stored in `wp_term_relationships`.  

---

## Summary Table

| Concept                | WordPress Equivalent | Database Table(s)                                 | Key Column / Meta Key                   |
|------------------------|----------------------|---------------------------------------------------|-----------------------------------------|
| Manga Series           | Custom Post Type     | `wp_posts`                                        | `post_type = 'wp-manga'`                |
| Manga Chapter          | Custom Post Type     | `wp_posts`                                        | `post_type = 'wp-chapter'`              |
| Series ↔ Chapter Link  | Parent–Child         | `wp_posts`                                        | `post_parent`                           |
| Series/Chapter Details | Post Meta            | `wp_postmeta`                                     | `meta_key` (e.g., `_wp_manga_status`)   |
| Genres & Tags          | Custom Taxonomy      | `wp_terms`, `wp_term_taxonomy`, `wp_term_relationships` | `taxonomy` (e.g., `manga-genre`) |

---

This structure provides a flexible, scalable manga-publishing platform that integrates seamlessly with the WordPress ecosystem.  
