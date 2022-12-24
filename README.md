
# Creating Custom ID using Custom Helper in a Laravel Application - `Helper Function with Custom ID` 

Laravel provides many excellent helper functions that are convenient for doing things like 
working with arrays, file paths, strings, and routes, among other things like the beloved 
dd() function. You can also define your own set of helper functions for your 
Laravel applications and PHP packages, by using Composer to import them automatically.

## Creating a Helpers file in a Laravel App for Unique ID - `Custom Helper` 



### Custom Helper - `Step 1`
- In the `app` folder, create a new folder called `Helpers`. In this folder, create a new file called `MyHelper.php` and add the following code to it
```bash
<?php

  function studentIDgenerator($model, $tableRowName, $length=4, $prefix)
  {
      $data = $model::orderBy('id', 'desc')->first();

      if(!$data)
      {
          $og_length      = $length;
          $last_number    = "";
      }
      else{
          $code                   = substr($data->$tableRowName, strlen($prefix)+1);
          $actual_last_number     = ($code/1)*1;
          $increment_last_number  = $actual_last_number+1;
          $last_number_length     = strlen($increment_last_number);
          $og_length              = $length - $last_number_length;
          $last_number            = $increment_last_number;
      }

      $zeros = "";
      for($i=0; $i<$og_length; $i++)
      {
          $zeros.="0";
      }  
      return $prefix.'-'.$zeros.$last_number;
  }
    
?>
```

### Custom Helper - `Step 2`
- Initialize your helper to declare it in `autoload` section. Go to `composer.json` and add the following code to it.
```bash
"autoload": {
        "psr-4": {
            "App\\": "app/",
            "Database\\Factories\\": "database/factories/",
            "Database\\Seeders\\": "database/seeders/"
        },
        "files": [
            "app/Helpers/Myhelper.php"
        ]
    },
```

### Custom Helper - `Step 3`
- Run the `CLI Command` to reload added `files` in `composer.json`.
```bash
CLI Command : composer dump-autoload
```

### Custom Helper - `Step 4`
- Use your decleared `function` in `MyHelper.php` in anywhere in the project. Like this : 
```bash
Demo : 
        public function createStudentInfo(Request $request)
          {
              $student = new Student();

              $student->student_id    = studentIDgenerator(new Student, 'student_id', 5, 'STD');
              $student->student_name  = $request->student_name;
              $student->student_status  = $request->student_status;
              $student->save();
              return $student;
          }
```

## Screenshots

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


# Documented by :  `Proshenjit Karmakar - Full Stack Developer`.
