# tfserving_client
__Python gRPC Client to access remote gRPC TensorFlow Server__
Protocol Buffer files borrowed from [tobegit3hub/deep_recommend_system](https://github.com/tobegit3hub/deep_recommend_system/tree/master/python_predict_client)

While creating a remote gRPC Client, came across [this issue](https://github.com/tensorflow/serving/issues/237) which didn't have a pure pythonic solution, instead going about through _Bazel_ compilation (Like the one described in TensorFlow Serving inference webpage).

Another solution: [sebastian-schlecht/tensorflow-serving-python](https://github.com/sebastian-schlecht/tensorflow-serving-python)

***

__How to run this (_Note: This runs only locally on Google Compute Engine, still working on remote_)__
- Create a VM (_any cloud provider_)
- Install Docker(_add user to sudo docker group_) and necessary python version (Here it is 3.5x)
- `docker pull quay.io/pratos/baseinception`
- We need to make sure that the tensorflow server is started. Follow the commands below:
    * `/work/serving/bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server`
    * `bazel-bin/tensorflow_serving/model_servers/tensorflow_model_server --port=9000 --model_name=inception --model_base_path=inception-export &> inception_log &`
- Clone the repository.
- `$ cd tfserving_client`
- Create a new environment: `conda env create -f tfserving_client.yml`
- `$ cd python_predict_client`
- `python predict_client.py --server localhost:9000 --image ./dog-1210559_960_720.jpg` (You can add any sample image here)
- You should get the inference in the following format:
```
The time required to do inference is 1.60
outputs {
  key: "classes"
  value {
    dtype: DT_STRING
    tensor_shape {
      dim {
        size: 1
      }
      dim {
        size: 5
      }
    }
    string_val: "Labrador retriever"
    string_val: "golden retriever"
    string_val: "Rottweiler"
    string_val: "Rhodesian ridgeback"
    string_val: "Chesapeake Bay retriever"
  }
}
outputs {
  key: "scores"
  value {
    dtype: DT_FLOAT
    tensor_shape {
      dim {
        size: 1
      }
      dim {
        size: 5
      }
    }
    float_val: 8.128775596618652
    float_val: 6.055893421173096
    float_val: 4.217767715454102
    float_val: 3.918299436569214
    float_val: 3.659740686416626
  }
}
	
```
