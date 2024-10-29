# Features
Daily Increment: Automatically increments the counter by 1 each day.
Animated Display: JavaScript-based animated counter display for a smooth, engaging user experience.
Shortcode Support: Display the counter anywhere on your site using the [daily_counter] shortcode.
Admin Settings Page: Admin panel to manually set the counter value.
# Installation
Download the plugin and place it in your WordPress /wp-content/plugins/ directory.
Activate the plugin through the WordPress admin panel.
The counter will start from the default value (376) or any manually set value.
Use the [daily_counter] shortcode to display the counter on any page or post.
# Usage
Shortcode: Use [daily_counter] in your posts or pages where you want the counter to appear.
Admin Panel: Go to Daily Counter Settings in the WordPress admin to update the counter manually.
Plugin Code Highlights
Automatic Increment: Uses a daily cron job to increment the counter by 1 every day.
JavaScript Animation: A smooth animation script incrementally displays the counter value when the page loads.
Admin Reset Option: Allows the admin to reset or set the counter manually, ideal for custom use cases.
# Code Example
<?php
// Initialize the counter on activation
function daily_counter_activate() {
    if (get_option('daily_counter_value') === false) {
        update_option('daily_counter_value', 376);
    }
}

// Increment counter by 1 per day
function daily_counter_update() {
    $current_value = get_option('daily_counter_value', 376);
    if (strtotime('today') > get_option('daily_counter_last_increment', strtotime('today'))) {
        $new_value = $current_value + 1;
        update_option('daily_counter_value', $new_value);
    }
}

# Contributing
Feel free to fork this repository, create a branch, and submit a pull request. For issues, please create a new issue ticket in the Issues tab.
