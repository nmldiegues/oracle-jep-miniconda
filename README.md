# oracle-jep-miniconda
[![Build Status](https://travis-ci.com/feedzai/oracle-jep-miniconda.svg?branch=master)](https://travis-ci.com/feedzai/oracle-jep-miniconda)

Docker container with the following software installed:
 * Oracle JDK 8
 * [JEP](https://github.com/ninia/jep)
 * [miniconda](https://conda.io/miniconda.html) with Python3 and the following packages:
    * numpy 
    * scipy 
    * pandas 
    * scikit-learn

Several environment variables are also set up to make it easier for your Java processes to leverage all of this:
```
ENV CONDA_HOME /root/miniconda3
ENV PATH $CONDA_HOME/bin:$PATH
ENV CONDA_ENVIRONMENT conda-environment
ENV JEP_LOCATION ${CONDA_HOME}/envs/${CONDA_ENVIRONMENT}/lib/python3.6/site-packages/jep
ENV JEP_JAR ${JEP_LOCATION}/jep-3.7.1.jar

# Most software that requires JEP will probably need to point to the dyn libs generated by the command above
ENV LD_LIBRARY_PATH ${JEP_LOCATION}:${LD_LIBRARY_PATH}

# Processes that must see the correct python must add this variable to LD_PRELOAD (i.e. `export LD_PRELOAD=$TO_PRELOAD`)
# We don't do it here since that breaks yum
ENV TO_PRELOAD ${CONDA_HOME}/envs/${CONDA_ENVIRONMENT}/lib/libpython3.6m.so
```

To run in the correct environment the easiest way is to do:
```
source activate $CONDA_ENVIRONMENT
export LD_PRELOAD=$TO_PRELOAD
java -cp <your-classpath>:$JEP_JAR <your java command>
```
