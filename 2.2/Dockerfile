FROM openshift/base-centos7

# This image provides a Ruby 2.2 environment you can use to run your Ruby
# applications.

MAINTAINER SoftwareCollections.org <sclorg@redhat.com>

EXPOSE 8080

ENV RUBY_VERSION 2.2

LABEL io.k8s.description="Platform for building and running Ruby 2.2 applications" \
      io.k8s.display-name="Ruby 2.2" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,ruby,ruby22"

RUN INSTALL_PKGS="rh-ruby22 rh-ruby22-ruby-devel rh-ruby22-rubygem-rake rh-ruby22-rubygem-bundler qt5-qtwebkit-devel ImageMagick-devel nodejs xorg-x11-server-Xvfb" && \
    curl -sL https://rpm.nodesource.com/setup_4.x | bash - && \
    yum install -y centos-release-scl epel-release && \
    yum install -y --setopt=tsflags=nodocs  $INSTALL_PKGS && rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    alternatives --install /usr/local/bin/qmake qmake /usr/bin/qmake-qt5 5

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Each language image can have 'contrib' a directory with extra files needed to
# run and build the applications.
COPY ./contrib/ /opt/app-root

RUN chown -R 1001:0 /opt/app-root

USER 1001

# Set the default CMD to print the usage of the language image
CMD $STI_SCRIPTS_PATH/usage
