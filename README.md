Calendar 
**Calendar Helper for Simple MVC Framework**

**Installation**

1) Copy Calendar folder to your Helpers folder

2) Setup a route for your calendar

``` 
Router::any('thefunction', 'Controllers\TheController@thefunction');
Router::any('thefunction/(:num)/(:num)', 'Controllers\TheController@thefunction');//if you want to have next and prev 

```


3) Put this in your controller 
    
``` 
use Helpers\Calendar\Calendar; 
```

4) For creating a calendar just create an object and pass 2 parameters year,month to the generate method
```
public function thefunction(){
    $calendar = new Calendar();
    echo $calendar->generate(2015,2); //this will create a simple calendar 2015 Feb
}

```

5) You can pass data to the calendar, using an associative array where the keys are the days you want to populate
and the value can be a link.

```
public function thefunction(){
    $calendar = new Calendar();
    $data = array(
        3  => 'http://myweb.com/event1',
        7  => 'http://myweb.com/event2',
        13 => 'http://myweb.com/event3',
        26 => 'http://myweb.com/event4'
    );

    echo $calendar->generate(2015,2,$data); //this will create a simple calendar 2015 Feb
}

```

6) You can customize your calendar with some configuration.

```
    public function thefunction(){

        $config = array(
                'start_day'    => 'saturday',
                'month_type'   => 'long',
                'day_type'     => 'short'
        );

        $calendar = new Calendar($config);

        echo $calendar->generate(); //this will create a simple calendar
    }

```

7) If you want to put next, prev links you must pass parameter like this thefunction($year = '', $month = ''), 
 *show_next_prev* must be set to TRUE , the *next_prev_url* should be the name of the route on step 2 if left empty the
current function is the default route

```
    public function thefunction($year = '', $month = ''){

        $config = array(
        'show_next_prev'  => TRUE,
        'next_prev_url'   => 'thefunction' //if empty then current function is used
        );


        $calendar = new Calendar($config);

        echo $calendar->generate($year,$month); //this will create a simple calendar 
    }

```

8) You can change its design by using either the string or array method

```
        $config['template'] = '

            {table_open}<table border="0" cellpadding="0" cellspacing="0">{/table_open}

            {heading_row_start}<tr>{/heading_row_start}

            {heading_previous_cell}<th><a href="{previous_url}">&lt;&lt;</a></th>{/heading_previous_cell}
            {heading_title_cell}<th colspan="{colspan}">{heading}</th>{/heading_title_cell}
            {heading_next_cell}<th><a href="{next_url}">&gt;&gt;</a></th>{/heading_next_cell}

            {heading_row_end}</tr>{/heading_row_end}

            {week_row_start}<tr>{/week_row_start}
            {week_day_cell}<td>{week_day}</td>{/week_day_cell}
            {week_row_end}</tr>{/week_row_end}

            {cal_row_start}<tr>{/cal_row_start}
            {cal_cell_start}<td>{/cal_cell_start}
            {cal_cell_start_today}<td>{/cal_cell_start_today}
            {cal_cell_start_other}<td class="other-month">{/cal_cell_start_other}

            {cal_cell_content}<a href="{content}">{day}</a>{/cal_cell_content}
            {cal_cell_content_today}<div class="highlight"><a href="{content}">{day}</a></div>{/cal_cell_content_today}

            {cal_cell_no_content}{day}{/cal_cell_no_content}
            {cal_cell_no_content_today}<div class="highlight">{day}</div>{/cal_cell_no_content_today}

            {cal_cell_blank}&nbsp;{/cal_cell_blank}

            {cal_cell_other}{day}{/cal_cel_other}

            {cal_cell_end}</td>{/cal_cell_end}
            {cal_cell_end_today}</td>{/cal_cell_end_today}
            {cal_cell_end_other}</td>{/cal_cell_end_other}
            {cal_row_end}</tr>{/cal_row_end}

            {table_close}</table>{/table_close}';
            $calendar = new Calendar($config);

            echo $calendar->generate(); 

```

```

    $config['template'] = array(
            'table_open'           => '<table class="calendar">',
            'cal_cell_start'       => '<td class="day">',
            'cal_cell_start_today' => '<td class="today">'
    );
     $calendar = new Calendar($config);

     echo $calendar->generate();
```

9) Here is a reference for the config array 

|Config  |    Default    |   Options |   Description|
|----------|----------|----------|-----------|
|template   |	None    |   None    |   A string or array containing your calendar template.|
|local_time |	time()  |   None    |   A Unix timestamp corresponding to the current time.|
|start_day  | 	sunday  |   Any week day (sunday, monday, tuesday, etc.)    |   Sets the day of the week the calendar should start on.|
|month_type |   long    |   long, short |   Determines what version of the month name to use in the header. long = January, short = Jan.|
|day_type   |   abr     |   long, short, abr    |   Determines what version of the weekday names to use in the column headers. long = Sunday, short = Sun, abr = Su.|
|show_next_prev |   FALSE   |   TRUE/FALSE (boolean)    |   Determines whether to display links allowing you to toggle to next/previous months. See information on this feature below.|
|next_prev_url  |   Function   |    A Route |	Sets the basepath used in the next/previous calendar links.|
|show_other_days    |   FALSE   |   TRUE/FALSE (boolean)    | Determines whether to display days of other months that share the first or last week of the calendar month. |
|loc    |   es  | es, en |   This change the language currently supported en: English , es: Spanish|


