NAME := practice
CXX ?= c++
CXXFLAGS := -Wall -Wextra -Werror -std=c++17

SRC_DIRS := src lib
SRC := $(foreach dir,$(SRC_DIRS),$(wildcard $(dir)/*.cpp))
OBJ_DIR := object
OBJ := $(patsubst %.cpp,$(OBJ_DIR)/%.o,$(SRC))

GLFW_DIR := libs/glfw
GLFW_BUILD_DIR := $(GLFW_DIR)/build
GLFW_LIB := $(GLFW_BUILD_DIR)/src/libglfw3.a
GLFW_INCLUDE := -I$(GLFW_DIR)/include

LDFLAGS := $(GLFW_LIB) -lX11 -lXrandr -lXi -lXxf86vm -lXcursor -lGL -lm -ldl -lpthread -lrt

INSIDE_DOCKER ?= 0
DOCKER_COMPOSE ?= docker compose

ifeq ($(INSIDE_DOCKER),1)
ALL_TARGET := $(NAME)
else
ALL_TARGET := docker-build
endif

.PHONY: all docker-build docker-shell clean fclean re

all: $(ALL_TARGET)

docker-build:
	$(DOCKER_COMPOSE) -f docker-compose.yml build builder
	$(DOCKER_COMPOSE) -f docker-compose.yml run --rm -e INSIDE_DOCKER=1 builder make $(NAME)

docker-shell:
	$(DOCKER_COMPOSE) -f docker-compose.yml run --rm -e INSIDE_DOCKER=1 builder /bin/bash

$(NAME): $(OBJ) $(GLFW_LIB)
	$(CXX) $(CXXFLAGS) $(OBJ) $(LDFLAGS) -o $@

$(OBJ_DIR)/%.o: %.cpp
	@mkdir -p $(dir $@)
	$(CXX) -c $(CXXFLAGS) $(GLFW_INCLUDE) $< -o $@

$(GLFW_LIB):
	rm -rf $(GLFW_BUILD_DIR)
	cmake -S $(GLFW_DIR) -B $(GLFW_BUILD_DIR) -DBUILD_SHARED_LIBS=OFF -DGLFW_BUILD_EXAMPLES=OFF -DGLFW_BUILD_TESTS=OFF -DGLFW_BUILD_DOCS=OFF -DGLFW_BUILD_WAYLAND=OFF
	cmake --build $(GLFW_BUILD_DIR)

clean:
	rm -rf $(OBJ_DIR)

fclean: clean
	rm -f $(NAME)

re: fclean all






