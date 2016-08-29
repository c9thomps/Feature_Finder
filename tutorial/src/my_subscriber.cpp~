#include <ros/ros.h>
#include <image_transport/image_transport.h>
#include <opencv2/imgproc/imgproc.hpp>
#include <stdio.h>
#include <cv.h>
#include <opencv2/highgui/highgui.hpp>
#include <cv_bridge/cv_bridge.h>

cv::Mat img;

void imageCallback(const sensor_msgs::ImageConstPtr& msg)
{
  try
  {
		//cv::Mat temp;
		img = cv_bridge::toCvShare(msg, "bgr8")->image;
    //temp = cv_bridge::toCvShare(msg, "bgr8")->image;
    //cv::cvtColor(temp, img, 6);
  }
  catch (cv_bridge::Exception& e)
  {
    ROS_ERROR("Could not convert from '%s' to 'bgr8'.", msg->encoding.c_str());
    img.release();
  }
}

int main(int argc, char **argv)
{
  ros::init(argc, argv, "image_listener");
  ros::NodeHandle nh;
  cv::namedWindow("view");
  cv::startWindowThread();
  image_transport::ImageTransport it(nh);
  image_transport::Subscriber sub = it.subscribe("camera/image/", 1, imageCallback);
  //image_transport::Subscriber sub = it.subscribe("camera/image", 1, imageCallback);

	cv::Mat img_print, img_scene;

  while(1){
    if(!img.empty()){
			img.copyTo(img_print);
			cv::cvtColor(img_print, img_scene, 6);
			img.release();

			cv::imshow("view", img_scene);
			cv::waitKey(5);
    }
  	ros::spinOnce();
  }
  cv::destroyWindow("view");
}
