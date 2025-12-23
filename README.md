# Books Review Portal - README

## 📚 Overview
Books Review Portal is a full-featured web application built with Laravel that allows users to browse, search, and review books. The platform provides insights into book popularity through ratings and review counts, with filtering options for different time periods. This project demonstrates advanced Laravel concepts and modern web development practices.

## ✨ Features

### Core Features
- **Book Catalog**: Browse an extensive collection of books with ratings and review counts
- **Smart Filtering**: View most popular books by:
  - Last month
  - Last six months
- **Advanced Search**: Search books by title with database-powered backend search
- **Review System**: Users can add and read reviews for any book
- **Rating System**: Comprehensive rating display and calculation

### Technical Features
- **Database Relationships**: Complex relationship management between books, reviews, and users
- **Query Scopes**: Elegant query scoping for filtering books by time periods and popularity
- **Rate Limiting**: API protection against abuse with configurable rate limits
- **Backend Search**: Server-side search functionality with optimized database queries
- **Responsive Design**: Built with Tailwind CSS for optimal viewing on all devices
- **Form Handling**: Secure and validated form submissions for reviews
- **Dynamic Content**: Real-time rating updates and review counts

## 🛠️ Technologies Used

- **Backend**: Laravel 12.x
- **Frontend**: Tailwind CSS, Blade Templates
- **Database**: MySQL with Eloquent ORM
- **API Protection**: Laravel Rate Limiter
- **Development Tools**: Composer, Artisan CLI

## 📁 Project Structure
```
books-review-portal/
├── app/
│   ├── Models/
│   │   ├── Book.php
│   │   ├── Review.php
│   │   └── User.php
│   ├── Http/
│   │   ├── Controllers/
│   │   └── Requests/
│   └── ...
├── database/
│   ├── migrations/
│   └── seeders/
├── resources/
│   ├── views/
│   └── ...
├── public/
└── ...
```

## 🔧 Installation & Setup

### Prerequisites
- PHP 8.1 or higher
- Composer
- MySQL 5.7 or higher
- Node.js & NPM (for Tailwind)

### Installation Steps
1. Clone the repository:
   ```bash
   git clone [repository-url]
   cd books-review-portal
   ```

2. Install PHP dependencies:
   ```bash
   composer install
   ```

3. Install NPM dependencies:
   ```bash
   npm install
   ```

4. Configure environment:
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```
   
   Update `.env` with your database credentials:
   ```env
   DB_DATABASE=books_review_portal
   DB_USERNAME=root
   DB_PASSWORD=your_password
   ```

5. Run migrations and seeders:
   ```bash
   php artisan migrate --seed
   ```

6. Build frontend assets:
   ```bash
   npm run build
   ```

7. Start development server:
   ```bash
   php artisan serve
   ```

## 🚀 Usage

### For Users
1. Browse books on the homepage
2. Use search bar to find specific books
3. Filter by popularity (last month/six months)
4. Click on any book to view details and reviews
5. Submit your own reviews and ratings

### For Developers
- The application demonstrates:
  - Complex Eloquent relationships
  - Advanced query scoping
  - Rate limiting implementation
  - Database optimization techniques
  - Form validation and security
  - Responsive UI design principles

## 📊 Database Design

### Key Relationships
- **Books ↔ Reviews**: One-to-Many
- **Users ↔ Reviews**: One-to-Many (if authentication implemented)

### Key Models
- **Book**: Title, description, publication details
- **Review**: Content, rating, timestamps
- **User**: Authentication and review ownership

## ⚙️ Advanced Features

### Query Scopes
The project implements sophisticated query scopes for efficient data filtering:

```php
// Example Query Scopes in Book model
public function scopePopularLastMonth($query)
{
    return $query->withCount(['reviews' => function ($q) {
        $q->where('created_at', '>=', now()->subMonth());
    }])->orderBy('reviews_count', 'desc');
}

public function scopePopularLastSixMonths($query)
{
    return $query->withCount(['reviews' => function ($q) {
        $q->where('created_at', '>=', now()->subMonths(6));
    }])->orderBy('reviews_count', 'desc');
}

public function scopeHighestRated($query, $months = null)
{
    return $query->withAvg(['reviews' => function ($q) use ($months) {
        if ($months) {
            $q->where('created_at', '>=', now()->subMonths($months));
        }
    }], 'rating')->orderBy('reviews_avg_rating', 'desc');
}
```

### Rate Limiting
Implementation of Laravel's rate limiting to protect API endpoints:

```php
// Rate limiting configuration in web.php or route files
Route::middleware(['throttle:reviews'])->group(function () {
    Route::post('/books/{book}/reviews', [ReviewController::class, 'store']);
});

// Custom rate limiter in AppServiceProvider
protected function configureRateLimiting()
{
    RateLimiter::for('reviews', function (Request $request) {
        return Limit::perHour(5)->by($request->user()?->id ?: $request->ip());
    });
    
    RateLimiter::for('search', function (Request $request) {
        return Limit::perMinute(30)->by($request->ip());
    });
}
```

## 🎯 Key Technical Achievements

1. **Complex Query Optimization**: Efficiently query books with aggregated review data using Eloquent scopes
2. **Rate Limiting Implementation**: Robust API protection with custom rate limiters
3. **Relationship Management**: Proper handling of database relationships in Laravel
4. **Search Implementation**: Backend search with partial matching and optimization
5. **Rating Calculations**: Dynamic rating updates and statistical analysis
6. **Security Measures**: Form validation, CSRF protection, and rate limiting
7. **Responsive Design**: Mobile-first approach with Tailwind CSS

## 📝 Learning Outcomes

This project demonstrates proficiency in:
- Advanced Laravel development with Eloquent ORM
- Database design and query optimization
- API security and rate limiting implementation
- Full-stack web application development
- User experience design principles
- Modern PHP development practices
- Performance optimization techniques

## 🔮 Future Enhancements

Potential features for expansion:
- User authentication and profile management
- Social sharing capabilities
- Book recommendation engine using machine learning
- RESTful API endpoints for mobile applications
- Advanced analytics dashboard for publishers
- Multi-language support
- Caching implementation for improved performance
- Real-time notifications using WebSockets

## 🧪 Testing

The project includes comprehensive testing:
```bash
# Run feature tests
php artisan test

# Run specific test suites
php artisan test --testsuite=Feature
php artisan test --testsuite=Unit
```



**Note**: This project was developed to showcase advanced Laravel skills including query scoping, rate limiting, and full-stack web development capabilities.

---

### Technical Highlights for CV:
- ✅ **Advanced Query Scopes**: Implemented dynamic filtering for time-based popularity rankings
- ✅ **Rate Limiting**: Custom rate limiters for API protection and abuse prevention  
- ✅ **Database Optimization**: Efficient Eloquent relationships with aggregate queries
- ✅ **Full-Stack Development**: Complete application from database design to UI implementation
- ✅ **Security Best Practices**: Form validation, CSRF protection, and API security measures

---
