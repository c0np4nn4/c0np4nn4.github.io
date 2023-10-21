+++
title = "CB23606, 4, Ultra Simple Interpreter and Compiler"
date = "2023-10-22"
+++

# TL;DR

- `Interpreter`
    - Driver: **Bookkeeping** tasks
    - Engine: Interpret(**execute**) the source program
- `Compiler`
    - Set-up Code: Prologue + Epilogue
    - Driver: **Bookkeeping** tasks
    - Engine: **generates** the `target code`

---

# TODO
- Rust로 작성한 Koch Curve Interpreter / Compiler 를 첨부할 에정
- Koch 곡선을 그리는 코드는 아래와 같이 완성함.
    ```rust
    use std::io;

    use druid::kurbo::BezPath;
    use druid::widget::{Flex, Painter};
    use druid::{AppLauncher, Color, Point, RenderContext, Widget, WindowDesc};

    fn main() {
        let main_window = WindowDesc::new(build_ui)
            .title("Koch Curve")
            .window_size((1000.0, 500.0));

        AppLauncher::with_window(main_window)
            .launch(()) // Launching the application with an initial state of unit type `()`
            .expect("Failed to launch application");
    }

    fn build_ui() -> impl Widget<()> {
        let painter = Painter::new(|ctx, _, _env| {
            let size = ctx.size();
            let mut path = BezPath::new();

            let start = Point::new(0.0, 400.0 / 2.0);
            let end = Point::new(size.width, 400.0 / 2.0);

            let mut input = String::new();

            println!("Please enter koch_curve depth (1-7):");

            io::stdin()
                .read_line(&mut input)
                .expect("Failed to read line");

            let number: usize = input.trim().parse().expect("Please type a number!");

            if number > 7 {
                panic!("Depth should be lower than 8");
            }

            println!("You typed: {}", number);

            draw_koch_curve(&mut path, start, end, number);

            ctx.stroke(path, &Color::WHITE, 10.0);
        });

        Flex::column().with_child(painter)
    }

    fn draw_koch_curve(path: &mut BezPath, start: Point, end: Point, depth: usize) {
        if depth == 0 {
            path.move_to(start);
            path.line_to(end);
        } else {
            let third = (end - start) / 3.0;
            let p1 = start + third;
            let p3 = start + 2.0 * third;

            let angle = 60f64.to_radians();
            let p2 = Point::new(
                p1.x + angle.cos() * (p3.x - p1.x) - angle.sin() * (p3.y - p1.y),
                p1.y + angle.sin() * (p3.x - p1.x) + angle.cos() * (p3.y - p1.y),
            );

            draw_koch_curve(path, start, p1, depth - 1);
            draw_koch_curve(path, p1, p2, depth - 1);
            draw_koch_curve(path, p2, p3, depth - 1);
            draw_koch_curve(path, p3, end, depth - 1);
        }
    }

    ```

