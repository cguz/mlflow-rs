# mlflow rust client

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://gitHub.com/cguz/)
[![Visual Studio](https://img.shields.io/badge/VisualCode-blue?&logo=visualstudio)](https://code.visualstudio.com/)
[![Rust](https://img.shields.io/badge/Rust-white?&logo=rust&logoColor=black)](https://www.rust-lang.org/)

Author: Cesar Augusto Guzman Alvarez [@cguz](https://github.com/cguz/)

## Description

This is a continuation of the [mlflow-rs](https://github.com/luleyleo/mlflow-rs) project, which is a Rust library providing access to the MLflow REST API. 

Thus, all the credits for the original author!.

# Example

[minmal.rs](examples/minimal.rs)
```rust
fn main() {
    const EXPERIMENT: &str = "My Experiment";
    let mut client = Server::new("http://127.0.0.1:5000/api");
    let experiment = client.get_experiment_by_name(EXPERIMENT)
        .map(|experiment| experiment.experiment_id)
        .or_else(|_| client.create_experiment(EXPERIMENT))
        .expect("Could neither get nor create the experiment");

    for i in 0..3 {
        println!("Executing run {}", i);
        let mut run = TrackingRun::new();
        run.log_param("i", &format!("{}", i));
        run.log_param("constant", "42");
        let mut rng = WyRand::new_seed(i);
        for s in 0..10 {
            let int: f64 = rng.generate::<u16>().into();
            let max: f64 = std::u16::MAX.into();
            let value = int / max;
            run.log_metric("rand", value, s);
        }
        run.submit(&mut client, &experiment)
            .expect("Could not submit the run");
    }
}
```

# State

The following parts of the API are implemented:

- [x] Experiments
    - [x] Create
    - [x] Read
    - [x] Update
- [x] Runs
    - [x] Create
    - [x] Read
    - [x] Update
    - [x] Search
- [ ] Logging
    - [x] Parameters
    - [x] Metrics
    - [ ] Artifacts
- [ ] Models
    - [ ] Create
    - [ ] Read
    - [ ] Update
- [x] Tags
