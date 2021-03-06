
CC                     = "gcc"
CFLAGS                 = ""
FPGA_BITSTREAM_FILE    = "pump_axi4.rbf"
DEVICE_TREE_DIRECTORY  = "uio_irq_sample"
DEVICE_TREE_FILE       = "uio_irq_sample.dts"
UIO_DEVICE_NAME        = "uio0"
UDMABUF4_DEVICE_NAME   = "udmabuf4"
UDMABUF5_DEVICE_NAME   = "udmabuf5"

desc "Install fpga and devicetrees"
task :install => ["/lib/firmware/#{FPGA_BITSTREAM_FILE}", DEVICE_TREE_FILE] do
  begin
    sh "dtbocfg.rb --install #{DEVICE_TREE_DIRECTORY} --dts #{DEVICE_TREE_FILE}"
  rescue => e
    print "error raised:"
    p e
    abort
  end
  device_file = "/dev/" + UIO_DEVICE_NAME
  if (File.exist?(device_file) == false)
    abort "can not #{device_file} installed."
  end
  File::chmod(0666, device_file)
  if (File.exist?("/dev/" + UDMABUF4_DEVICE_NAME) == false)
    abort "can not udmabuf installed."
  end
  File::chmod(0666, "/dev/" + UDMABUF4_DEVICE_NAME)
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_mode")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_offset")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_size")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_direction")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_owner")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_for_cpu")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF4_DEVICE_NAME + "/sync_for_device")
  if (File.exist?("/dev/" + UDMABUF5_DEVICE_NAME) == false)
    abort "can not udmabuf installed."
  end
  File::chmod(0666, "/dev/" + UDMABUF5_DEVICE_NAME)
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_mode")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_offset")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_size")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_direction")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_owner")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_for_cpu")
  File::chmod(0666, "/sys/class/udmabuf/" + UDMABUF5_DEVICE_NAME + "/sync_for_device")
end

desc "Uninstall fpga and devicetrees"
task :uninstall do
  device_file = "/dev/" + UIO_DEVICE_NAME
  if (File.exist?(device_file) == false)
    abort "can not #{device_file} uninstalled: does not already exists."
  end
  sh "dtbocfg.rb --remove #{DEVICE_TREE_DIRECTORY}"
end

file "/lib/firmware/#{FPGA_BITSTREAM_FILE}" => ["#{FPGA_BITSTREAM_FILE}"] do
  sh "cp #{FPGA_BITSTREAM_FILE} /lib/firmware/#{FPGA_BITSTREAM_FILE}"
end

file "/dev/#{UIO_DEVICE_NAME}" do
  Rake::Task["install"].invoke
end

file "sample1" => ["sample1.c", "sample_common.h"] do
  sh "#{CC} #{CFLAGS} -o sample1 sample1.c"
end
  
file "sample2" => ["sample2.c", "sample_common.h"] do
  sh "#{CC} #{CFLAGS} -o sample2 sample2.c"
end
  
task :default => ["/dev/#{UIO_DEVICE_NAME}", "sample1", "sample2"]
