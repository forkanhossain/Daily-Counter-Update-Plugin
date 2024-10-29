<?php
/*
Plugin Name: Daily Counter Update
Plugin URI: 
Description: A counter that increments each day and saves it in the database, with animated display.
Version: 1.0.0
Author: WebCityLab
Author URI: https://webcitylab.com/
*/

if (!defined('ABSPATH')) exit; // Exit if accessed directly

// Initialize the counter and timestamp on plugin activation
function daily_counter_activate() {
    if (get_option('daily_counter_value') === false) {
        update_option('daily_counter_value', 376);
    }
    if (get_option('daily_counter_last_increment') === false) {
        update_option('daily_counter_last_increment', strtotime('today'));
    }
}
register_activation_hook(__FILE__, 'daily_counter_activate');

// Increment counter by 1 per day if a new day has started
function daily_counter_update() {
    $current_value = get_option('daily_counter_value', 376);
    $last_increment = get_option('daily_counter_last_increment', strtotime('today'));
    
    // Check if a new day has started
    if (strtotime('today') > $last_increment) {
        $increment_amount = 1; 
        $new_value = $current_value + $increment_amount;
        update_option('daily_counter_value', $new_value);
        update_option('daily_counter_last_increment', strtotime('today'));
    }
}
add_action('init', 'daily_counter_update'); // Run on each page load

// Shortcode to display the animated counter
function daily_counter_display() {
    $current_value = get_option('daily_counter_value', 376); // Default to 376 if option is missing
    $formatted_value = number_format($current_value, 0, '', ',');

    // Output counter with JavaScript for animation
    $output = "
        <div id='daily-counter'>
            <p class='approved-merchants-counter' data-count='{$formatted_value}'>1</p>
        </div>
        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const counterElement = document.querySelector('.approved-merchants-counter');
                const targetCount = parseInt(counterElement.getAttribute('data-count').replace(',', ''));
                let count = 1;
                const duration = 2000;
                const increment = Math.ceil(targetCount / (duration / 10));

                function updateCounter() {
                    count += increment;
                    if (count > targetCount) count = targetCount;
                    counterElement.textContent = count.toLocaleString();

                    if (count < targetCount) {
                        setTimeout(updateCounter, 10);
                    }
                }
                updateCounter();
            });
        </script>
    ";
    
    return $output;
}
add_shortcode('daily_counter', 'daily_counter_display');

// Optional: Add admin menu for manual counter reset
function daily_counter_menu() {
    add_menu_page('Daily Counter Settings', 'Daily Counter Settings', 'manage_options', 'daily-counter', 'daily_counter_settings_page');
}
add_action('admin_menu', 'daily_counter_menu');

// Settings page content
function daily_counter_settings_page() {
    if (isset($_POST['set_counter_value'])) {
        update_option('daily_counter_value', intval($_POST['counter_value']));
        update_option('daily_counter_last_increment', strtotime('today'));
        echo "<div class='updated'><p>Counter value updated!</p></div>";
    }

    $current_value = get_option('daily_counter_value', 376);
    ?>
    <div class="wrap">
        <h1>Daily Counter Settings</h1>
        <form method="post" action="">
            <label for="counter_value">Set Counter Value:</label>
            <input type="number" name="counter_value" value="<?php echo esc_attr($current_value); ?>" />
            <input type="submit" name="set_counter_value" value="Set Counter" class="button button-primary" />
        </form>
        <p>Add this shortcode [daily_counter] where you want to show the counter</p>
    </div>
    <?php
}
